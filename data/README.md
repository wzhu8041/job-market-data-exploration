# Data folder

This project uses U.S. Bureau of Labor Statistics (BLS) flat files.

## Expected layout

- `data/raw/` – original BLS downloads (large). This folder is **gitignored** by default.
- `data/processed/` – small derived files you *can* commit (e.g., a single unemployment series).

## Files used in the notebook

JOLTS (Job Openings and Labor Turnover Survey):
- `jt.data.2.JobOpenings.txt`
- `jt.data.3.Hires.txt`
- `jt.data.5.Quits.txt`

CES (Current Employment Statistics):
- `ce.data.55a.FinancialActivities.Employment.txt`
- `ce.data.55b.FinancialActivities.AllEmployeeHoursAndEarnings.txt`
- `ce.data.60a.ProfessionalBusinessServices.Employment.txt`
- `ce.data.60b.ProfessionalBusinessServices.AllEmployeeHoursAndEarnings.txt`

Unemployment (CPS):
- `ln.data.1.AllData.txt` (very large). In practice, it’s best to pre-filter this to only the U-3 series (`LNS14000000`) and commit a small CSV.

If you don’t want to download the full CPS file, add a `data/processed/unemployment_u3.csv` with columns: `date,ur`.
