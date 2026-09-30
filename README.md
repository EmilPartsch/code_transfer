"""
Replicate Table D.0.2: decomposition of the change in the fiscal sustainability indicator (rHBI)
for permanent, unfinanced increases in structural employment by age group.

Run from Analysis/Standard_shocks after shock_labour_supply_by_age.gms.

Decomposition (paper, Appendix A), with N = numerator of rHBI and Y = PV of GDP:
    dS = dN / Y_s  +  N_b * (1/Y_s - 1/Y_b)
         ^ budget items    ^ "Share of change from GDP"
Each budget item's share is its (sign-adjusted) change in present value divided by Y_s and by dS.
"""
import dreamtools as dt
import pandas as pd

T = 2030  # Shock year = year in which rHBI is evaluated
GDX = "Gdx"
SHOCKS = {
    "20-29": "Arbejdsudbud_alder_20_29_ufin",
    "30-39": "Arbejdsudbud_alder_30_39_ufin",
    "40-49": "Arbejdsudbud_alder_40_49_ufin",
    "50-59": "Arbejdsudbud_alder_50_59_ufin",
    "60-69": "Arbejdsudbud_alder_60_69_ufin",
    # "1% (Table 4.2.2)": "Arbejdsudbud_beskaeftigelse_ufin",  # useful sanity check
}


def get(db, name, idx=None, suffix=""):
    """Value in year T of a variable (optionally one element of its first index)."""
    x = getattr(db, name + suffix)
    if idx is not None:
        x = x.xs(idx, level=0)
    return float(x.loc[T])


def items(db, suffix=""):
    """Present values (in year T) that enter rHBI, and rHBI's numerator."""
    g = lambda name, idx=None: get(db, name, idx, suffix)
    d = {
        "Y": g("nvBNP"),
        "revenues": g("nvOffPrimInd"),
        "direct_taxes": g("nvtDirekte"),
        "expenditures": g("nvOffPrimUd"),
        "gov_consumption": g("nvG", "gTot"),
        "transfers": g("nvOvf", "tot"),
        "investments": g("nvOffInv"),
        "subsidies": g("nvOffSub"),
        "other_exp": g("nvOffUdRest"),
        "omv": g("nvOffOmv"),
        "rentemarginal": g("nvRenteMarginal"),
        "rHBI": g("rHBI"),
    }
    # Initial net assets term: vOff13Net[T-1]/fv * (1+mrOffRente[T+1])
    fv = float(db.fv)
    debt = float(getattr(db, "vOff13Net" + suffix).loc[T - 1]) / fv * (1 + float(getattr(db, "mrOffRente" + suffix).loc[T + 1]))
    d["N"] = g("nvPrimSaldo") + d["omv"] - d["rentemarginal"] + debt
    d["other_rev"] = d["revenues"] - d["direct_taxes"]
    assert abs(d["N"] / d["Y"] - d["rHBI"]) < 1e-6, "Recomputed S does not match rHBI"
    return d


def reference(s, fallback):
    """Use the baseline re-solved inside the shock run (<var>_baseline parameters) if available."""
    try:
        return items(s, "_baseline")
    except (AttributeError, KeyError, AssertionError):
        return items(fallback)


def decompose(b, s):
    dS = s["N"] / s["Y"] - b["N"] / b["Y"]
    share = lambda x, sign=1: sign * (s[x] - b[x]) / s["Y"] / dS
    rows = {
        "Change in fiscal sustainability indicator (dS), %-points": 100 * dS,
        "Share of change from GDP": b["N"] * (1 / s["Y"] - 1 / b["Y"]) / dS,
        "Share of change from primary expenditures (total)": share("expenditures", -1),
        "  Government consumption": share("gov_consumption", -1),
        "  Transfers": share("transfers", -1),
        "  Public investments": share("investments", -1),
        "  Subsidies": share("subsidies", -1),
        "  Other expenditures": share("other_exp", -1),
        "Share of change from primary revenues (total)": share("revenues"),
        "  Direct taxes": share("direct_taxes"),
        "  Other revenues": share("other_rev"),
        "Share from revaluations and interest margin (not in paper)": share("omv") - share("rentemarginal"),
    }
    total = sum(v for k, v in rows.items() if k.startswith("Share"))
    assert abs(total - 1) < 1e-6, f"Shares sum to {total}, not 100%"
    return rows


if __name__ == "__main__":
    # dS is tiny (~0.0003), so the reference must be the baseline re-solved with the same terminal year as the shocks.
    # Preferred: <var>_baseline parameters in the shock GDX; else the zero shock; else baseline.gdx (with a warning).
    import os
    if os.path.exists(f"{GDX}/Nulstoed_ufin.gdx"):
        b_file = dt.Gdx(f"{GDX}/Nulstoed_ufin.gdx")
    else:
        print("WARNING: using baseline.gdx as fallback reference - check that its terminal year matches the shocks.")
        b_file = dt.Gdx(f"{GDX}/baseline.gdx")
    table = {}
    for label, name in SHOCKS.items():
        s = dt.Gdx(f"{GDX}/{name}.gdx")
        table[label] = decompose(reference(s, b_file), items(s))
    table = pd.DataFrame(table)

    fmt = table.copy().astype(object)
    fmt.iloc[0] = table.iloc[0].map("{:.4f}".format)
    fmt.iloc[1:] = table.iloc[1:].map("{:.0%}".format)
    print(fmt.to_string())
    table.to_csv("table_D02.csv")
