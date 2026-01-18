# U.S. Job Market Data Exploration (BLS JOLTS + CES)

A lightweight portfolio project exploring U.S. labor-market conditions using **BLS JOLTS** (openings/hires/quits), the **unemployment rate (U‑3)**, and **BLS CES** (sector employment & hourly earnings).

The repo is intentionally simple: one notebook + a handful of figures + this README to interpret what’s going on.

**Notebook:** [`notebooks/Job_Market_Data_Exploration.ipynb`](notebooks/Job_Market_Data_Exploration.ipynb)

---

## TL;DR (what the charts say)

- **Cooling since 2023:** the Beveridge curve shifts toward **lower openings and higher unemployment** versus 2023.
- **Job seekers’ environment deteriorated:** the Job Hunt Index trends from strongly positive in 2022 to **deeply negative by late‑2025**.
- **The latest drop is mostly “hiring momentum” slowing:** recent changes are dominated by **hires** (and secondarily openings / unemployment).
- **Business & finance sectors:** **wage growth stays ~4–5% YoY** even as **employment growth is flat to slightly negative**.

---

## Questions I’m trying to answer

1. **How “tight” is the labor market?** (vacancies vs. unemployment)
2. **How might the market *feel* to a job seeker?** (a composite “Job Hunt Index”)
3. **Where are jobs and wage growth concentrated?** (selected CES industries)

---

## Repo contents

- Notebook: `notebooks/Job_Market_Data_Exploration.ipynb`
- Figures used in this README: `figures/*.png`
- Data notes + expected file names: `data/README.md`

## Data & tools

**Data sources (BLS flat files)**
- **JOLTS**: job openings, hires, quits
- **CPS / Labor Force**: unemployment rate (U‑3)
- **CES**: employment + average hourly earnings by industry

**Stack**: Python, pandas, Plotly (optional: Kaleido to export PNGs).

> Note: The full CPS unemployment flat file is huge. In practice, it’s best to filter it down to the single U‑3 series (`LNS14000000`) and commit a small CSV (or keep the raw file gitignored).

---

## Visual 1 — Beveridge Curve (Unemployment vs Job Openings)

![Beveridge curve](figures/beveridge_data_vis.png)

**What this shows**
- Each point is a month.
- X-axis: **unemployment rate (%)**
- Y-axis: **job openings rate (%)**

**How to read it**
- A “hot” labor market typically sits in the **upper-left** (high openings + low unemployment).
- Moving **down/right** generally indicates cooling conditions (openings fall, unemployment rises).

**What I see in 2021–2025**
- 2023 clusters at **higher openings (~5–6%+)** with **lower unemployment (~3.4–3.9%)**.
- By 2024–2025, points shift to **lower openings (~4.3–4.8%)** with **higher unemployment (~4.0–4.5%)**.
- The last 6 months in 2025 are highlighted, showing the market sitting in a noticeably cooler region than 2023.

### Sector demand check — Job openings rate by industry

![Openings rate by industry](figures/job_opening_rate_data_vis.png)

This line chart zooms into job-openings rates for two white-collar sectors:
- **Professional & business services (PBS)** tends to run **higher** than **financial activities** in this window.
- Both series look **lower/softer by late‑2025** than their peaks earlier in the period (especially financial activities).

---

## Visual 2 — “Job Hunt Index” (a simple composite)

![Job Hunt Index](figures/job_hunt_index_data_vis.png)

I built a simple index to summarize how the job market might *feel* to job seekers:

```text
Job Hunt Index = z(Openings Rate) + z(Hires Rate) + z(Quits Rate) - z(Unemployment Rate)
```

- `z(x)` is the z-score within the analysis window.
- Higher values ≈ easier conditions (more openings/hires/quits, lower unemployment).

**Interpretation**
- The index trends steadily downward from 2022 → 2025, which is consistent with a market that’s moved from “very tight” to much more normal (and, by late‑2025, job-seeker‑unfriendly).

### What moved the index most recently?

![Index change contributions](figures/moved_index_data_vis.png)

This chart decomposes the latest index change (Sep → Nov 2025 in my run) into **component contributions**.

- The largest negative contribution comes from **hires**, suggesting job-finding momentum weakened.
- **Openings** and **unemployment** also contribute negatively.
- **Quits** shows no visible bar here because it didn’t meaningfully change over that particular step (so its contribution is ~0).

---

## Visual 3 — CES snapshots (employment vs wage growth)

The goal here is to connect “macro” cooling to what’s happening in **pay** and **headcount** for two sectors:
- **Financial activities**
- **Professional & business services**

### Snapshot table (year-end)

![Year-over-year snapshot table](figures/year_over_year_table_data_vis.png)

A quick read of the latest snapshot (Dec 2025 in the figure):
- **Financial activities:** employment **+0.41% YoY**, hourly earnings **+4.66% YoY**
- **Professional & business services:** employment **-0.43% YoY**, hourly earnings **+4.02% YoY**

### Side-by-side YoY heatmap (2023 → 2025)

![CES YoY heatmap](figures/year_over_year_data_vis.png)

**What stands out**
- Employment growth is **flat to slightly negative** (especially in PBS), but
- Wage growth remains **solid (~4–5% YoY)** across years.

One way to read this: firms may be **holding the line on compensation** while being more cautious about **net headcount expansion**.

---

## How to run

1. Create and activate a virtual environment
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Place BLS flat files in `data/raw/` (or commit smaller processed versions in `data/processed/`).
4. Open and run the notebook in `notebooks/`.

---

## Repo layout

```
.
├── notebooks/
├── figures/
├── data/
│   ├── raw/          # gitignored (large BLS files)
│   └── processed/    # small derived files you can commit
├── requirements.txt
└── README.md
```

---

## Possible extensions

- Add more industries (e.g., Information, Healthcare, Government).
- Swap plain z-scores for **rolling** z-scores to emphasize regime shifts.
- Package the visuals into a simple dashboard (Streamlit) or a Quarto report.
