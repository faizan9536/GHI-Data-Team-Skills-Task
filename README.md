# GHI Data Team Skills Task — HIV 2015 Impact Score

This repo reproduces the **HIV 2015 ORS Impact Score** calculation in Python and exports the required `impact_score.csv`.

## What’s included
- `notebooks/HIV_2015_impact_score.ipynb` — end-to-end, **validated** notebook (Excel-matching).
- `scripts/run_hiv2015.py` — command-line script that generates `impact_score.csv` without Jupyter.
- `requirements.txt` — minimal dependencies.
- (Output) `impact_score.csv` — generated after running the notebook or the script.

## Quickstart

### 1) Create environment
```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
source .venv/bin/activate
pip install -r requirements.txt
```

### 2) Place the ORS workbook
Put the workbook here (recommended):
```
data/ORS_2015_2017_2019.xlsx
```
> The assignment instructions say not to edit shared Google files. Make a local copy first.

### 3) Run (script)
```bash
python scripts/run_hiv2015.py --xlsx data/ORS_2015_2017_2019.xlsx --sheet HIV2015 --out impact_score.csv
```

### 4) Run (notebook)
Open `notebooks/HIV_2015_impact_score.ipynb`, set `xlsx_path`, run all cells.
It will export `impact_score.csv`.

## Method summary 
The HIV impact model computes DALYs averted by each drug by:
1. Splitting treatment into quadrants (adult/child × first/second line) using regimen tables.
2. Summing contributions from regimens that include a given drug.
3. Normalizing by retention rate (effective treatment length) as described in the supporting info documentation.

The implementation here is **header-anchored** (entity map style) so that every term can be traced back to the ORS sheet.

## Notes / assumptions
- The sheet is loaded as a raw grid (`header=None`) to mirror Excel’s layout.
- Percent-like cells are normalized to fractions (e.g., `72` → `0.72`) where needed.
- Where the ORS provides a regimen-size/denominator column, it is used; otherwise regimen size is inferred from the regimen string.

## Reproducibility
This code was validated by directly comparing per-country, per-drug values vs Excel columns, with max absolute differences on the order of ~1e-6 (floating tolerance).
