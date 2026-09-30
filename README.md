# ======================================================================================================================
# Replication of Table D.0.2:
# Permanent, unfinanced increase in structural employment of 1,000 persons, by age group
# ======================================================================================================================
$onDotL # Allow implicit .l suffix
$SETLOCAL shock_year 2030;
$SETLOCAL baseline_end 2129;

set_time_periods(%shock_year%-1, %baseline_end%);
@load_as(All, "Gdx/baseline.gdx", _baseline)
$GROUP All_ All; # to avoid foreign variables
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
  n_persons    "Additional structurally employed persons" /1000/
  persons_unit "Value of ONE person in the units of snLHh: 1 = persons, 1e-3 = thousands, 1e-6 = millions" /1e-6/
  per_age      "1: n_persons added at EACH age in the group; 0: n_persons in TOTAL for the group" /0/
  age_lo       "Lowest age in shocked group"
  age_hi       "Highest age in shocked group"
;
parameter
  permanent_profile[t] "Permanent shock profile"
  group_total[t]       "Baseline structural employment in the shocked age group"
;
permanent_profile[t]$(tx0[t]) = 1;

# ----------------------------------------------------------------------------------------------------------------------
# Age groups (columns of Table D.0.2). Comment out groups you do not want to run.
# ----------------------------------------------------------------------------------------------------------------------
$FOR {label}, {lo}, {hi} in [
  ("20_29", 20, 29),
  ("30_39", 30, 39),
  ("40_49", 40, 49),
  ("50_59", 50, 59),
  ("60_69", 60, 69),
]:
  Model M_shock /M_base/;
  @set(All_, .l, _baseline);
  $GROUP G_shock_endo G_endo;

  age_lo = {lo};
  age_hi = {hi};
  group_total[t] = sum(a$(aVal[a] >= age_lo and aVal[a] <= age_hi), snLHh[a,t]);
  display "Check units of snLHh against persons_unit:", group_total;

  # Add n_persons to structural employment in the group.
  # per_age = 0: spread proportionally to baseline employment, so the group total rises by exactly n_persons
  # per_age = 1: add n_persons at each age
  snLHh[a,t]$(tx0[t] and aVal[a] >= age_lo and aVal[a] <= age_hi and group_total[t] > 0)
    = snLHh[a,t]
    + n_persons * persons_unit * permanent_profile[t]
      * (per_age + (1 - per_age) * snLHh[a,t] / group_total[t]);

  # Structural employment exogenous (shocked), participation parameter endogenous - as in Arbejdsudbud_beskaeftigelse
  $GROUP+ G_shock_endo
    -snLHh[a,t]$(tx0[t] and aVal[a] >= {lo} and aVal[a] <= {hi}),
    uDeltag[a,t]$(tx0[t] and aVal[a] >= {lo} and aVal[a] <= {hi});

  # Unfinanced: no tax reaction block
  # Solve in two steps (1/100 of shock first) as in the standard shock file
  @set_linear_combination(All, 0.01, .l, _baseline);
  $FIX All; $UNFIX G_shock_endo;
  @solve(M_shock)
  @set_linear_combination(All, 100, .l, _baseline);
  $FIX All; $UNFIX G_shock_endo;
  @solve(M_shock)

  $UNFIX All;
  @unload(Gdx/Arbejdsudbud_alder_{label}_ufin);
$ENDFOR
