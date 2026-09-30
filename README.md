# ======================================================================================================================
# Replication of Table D.0.2:
# Permanent, unfinanced increase in structural employment of 1,000 persons, by age group
#
# Built on "Arbejdsudbud_beskaeftigelse" in standard_shocks.gms: structural employment (snLHh) is exogenous and
# shocked, and the participation parameter (uDeltag) is endogenous for all ages 15-100.
# Only ages inside the shocked group are changed; all other ages stay at baseline employment.
#
# Units (checked in Model/Gdx/baseline.gdx): snLHh is in 1,000 persons (snLHh[tot,2030] = 3,088).
# 1,000 persons = 1 unit of snLHh.
# ======================================================================================================================
$onDotL # Allow implicit .l suffix
$SETLOCAL shock_year 2030;
$SETLOCAL baseline_end 2129;

set_time_periods(%shock_year%-1, %baseline_end%);
@load_as(All, "Gdx/baseline.gdx", _baseline)
$GROUP All_ All;
@unload(Gdx/shock_year.gdx);
@load_dummies(t, "Gdx/baseline.gdx")

# Solve M_base with %baseline_end% as terminal year and use that solution as baseline
@set(All_, .l, _baseline);
$FIX All; $UNFIX G_endo;
@solve(M_base);
@set(All_, _baseline, .l);
$UNFIX All;

OPTION SOLVELINK=0, NLP=CONOPT4;

# ----------------------------------------------------------------------------------------------------------------------
# Shock settings
# ----------------------------------------------------------------------------------------------------------------------
scalar
  shock_persons "Extra structurally employed, in 1,000 persons (units of snLHh)" /1/
  per_age       "1: shock_persons added at EACH age in the group (10 ages -> 10,000 persons);
                 0: shock_persons in TOTAL for the group, spread by baseline employment" /1/
  move_soc      "0: transfer groups (snSoc) unchanged, as in the standard shocks;
                 1: move people out of transfer groups using dSoc2dBesk" /0/
;
parameter
  permanent_profile[t] "Permanent shock profile"
  group_total[t]       "Baseline structural employment in the shocked age group, 1,000 persons"
  d_snLHh_tot[t]       "Shock to total structural employment, 1,000 persons"
;
permanent_profile[t]$(tx0[t]) = 1;

# ----------------------------------------------------------------------------------------------------------------------
# Age groups = columns of Table D.0.2
# ----------------------------------------------------------------------------------------------------------------------
$FOR1 {label}, {lo}, {hi} in [
  ("20_29", 20, 29),
  ("30_39", 30, 39),
  ("40_49", 40, 49),
  ("50_59", 50, 59),
  ("60_69", 60, 69),
]:
  Model M_shock /M_base/;
  @set(All_, .l, _baseline);

  # Same endogeneity as Arbejdsudbud_beskaeftigelse: snLHh exogenous, uDeltag endogenous, for ALL ages.
  # Ages outside the group therefore stay exactly at baseline employment.
  $GROUP G_shock_endo
    G_endo
    -snLHh[a,t]$(tx0[t] and a15t100[a]), uDeltag[a,t]$(tx0[t] and a15t100[a])
  ;

  group_total[t] = sum(a$(aVal[a] >= {lo} and aVal[a] <= {hi}), snLHh[a,t]);

  snLHh[a,t]$(tx0[t] and aVal[a] >= {lo} and aVal[a] <= {hi})
    = snLHh[a,t]
    + shock_persons * permanent_profile[t]
      * (per_age + (1 - per_age) * snLHh[a,t] / group_total[t]);

  d_snLHh_tot[t] = sum(a$a15t100[a], snLHh[a,t] - snLHh_baseline[a,t]);
  display "Shock to total structural employment (1,000 persons):", d_snLHh_tot;

  # Optional: take the new workers out of transfer groups in the same proportions as the model uses cyclically
  snSoc[soc,t]$(tx0[t] and move_soc) = snSoc[soc,t] + dSoc2dBesk[soc,t] * d_snLHh_tot[t];

  # Unfinanced: no tax reaction. Solve 1/100 of the shock first, then the full shock.
  @set_linear_combination(All, 0.01, .l, _baseline);
  $FIX All; $UNFIX G_shock_endo;
  @solve(M_shock)
  @set_linear_combination(All, 100, .l, _baseline);
  $FIX All; $UNFIX G_shock_endo;
  @solve(M_shock)

  $UNFIX All;
  @unload(Gdx/Arbejdsudbud_alder_{label}_ufin);
$ENDFOR1
