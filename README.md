# coding: utf-8
"""
Pull qBNP and CPI responses for a set of shocks out of the gdx files and save them as csv.

Run from Analysis/Standard_shocks (same folder as the plotting script).
Output: one wide csv (rows = shock x variable, columns = years) and one long csv.
"""
import os
import glob
import dreamtools as dt
import pandas as pd

# ----------------------------------------------------------------------------------------------------------------------
# Settings
# ----------------------------------------------------------------------------------------------------------------------
gdx_dir = "./Gdx"
baseline_path = f"{gdx_dir}/baseline.gdx"
output_dir = "Output/tables"

# Years to include in the table (every year)
years = list(range(2029, 2046))

# Shocks and variations to include - gdx files are named {shock}_{variation}.gdx
shock_names = [
    "Offentligt_forbrug",
]
variations = [
    "midl",
    "perm",
    "blip",
    "perm_annonceret",
]
shocks = [f"{shock}_{variation}" for shock in shock_names for variation in variations]
# Set shocks = None to take every shock gdx in the folder instead.

# Files in Gdx/ that are not shocks
exclude = {"baseline", "shock_year", "Nulstoed_ufin", "Nulstoed_ufin_annonceret"}

# Variables: label -> (function of database, operator)
#   "pq" = pct. change relative to baseline, "m" = absolute difference to baseline
variables = {
    "qBNP": (lambda db: db.qBNP, "pq"),
    "CPI": (lambda db: db.pC["cTot"], "pq"),  # Private consumption deflator as CPI. Swap for your CPI variable if you use another.
}

# ----------------------------------------------------------------------------------------------------------------------
# Load baseline
# ----------------------------------------------------------------------------------------------------------------------
dt.time(min(years), max(years))
b = dt.REFERENCE_DATABASE = dt.Gdx(baseline_path)

if shocks is None:
    shocks = sorted(
        os.path.splitext(os.path.basename(p))[0]
        for p in glob.glob(f"{gdx_dir}/*.gdx")
        if os.path.splitext(os.path.basename(p))[0] not in exclude
    )

def compare(shock_series, baseline_series, operator):
    s = shock_series.loc[years]
    r = baseline_series.loc[years]
    if operator == "pq":
        return (s / r - 1) * 100
    if operator == "m":
        return s - r
    raise ValueError(f"Unknown operator {operator}")

# ----------------------------------------------------------------------------------------------------------------------
# Build table
# ----------------------------------------------------------------------------------------------------------------------
rows = []
for shock in shocks:
    path = f"{gdx_dir}/{shock}.gdx"
    if not os.path.exists(path):
        print(f"Skipping {shock}: {path} not found")
        continue
    s = dt.Gdx(path)
    for label, (func, operator) in variables.items():
        values = compare(func(s), func(b), operator)
        for year, value in values.items():
            rows.append({"shock": shock, "variable": label, "unit": operator, "year": int(year), "value": float(value)})

long = pd.DataFrame(rows)
wide = long.pivot_table(index=["shock", "variable", "unit"], columns="year", values="value").reset_index()

os.makedirs(output_dir, exist_ok=True)
long.to_csv(f"{output_dir}/shock_qBNP_CPI_long.csv", index=False)
wide.to_csv(f"{output_dir}/shock_qBNP_CPI.csv", index=False, float_format="%.4f")

print(wide.to_string(index=False))
