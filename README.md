# GHI Data Team Skills Task — HIV 2015 Impact Score (HIV2015 ORS)

This submission reproduces the **HIV 2015 ORS Impact Score** calculation in Python and provides the required output file: **`impact_score.csv`**.

The workflow mirrors the process shown in the HIV 2013 reference notebook and the Entity Map guide, and implements the HIV Impact Score formula as described in the ORS supporting documentation (example calculations / model reference).

---

## Files in This Submission

- **`HIV_2015_impact_score_refined.ipynb`** — end-to-end notebook (load → entity map / anchors → compute → validate → export)
- **`impact_score.csv`** — final output (one row per country; one column per drug as `Impact Score (<drug>)`)
- **`requirements.txt`** — minimal Python dependencies to run the notebook
- **`.gitignore`** — excludes local-only folders/files (virtualenv, caches, local data copies)

---

## How to Reproduce the Results

### 1) Environment setup
Create and activate a virtual environment (recommended), then install dependencies:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
source .venv/bin/activate
pip install -r requirements.txt
```

### 2) ORS workbook
As instructed in the assignment, use a **local copy** of the ORS workbook (do not edit shared Google files).

### 3) Run the notebook
Open **`HIV_2015_impact_score_refined.ipynb`**, then set:

```python
xlsx_path = "path/to/your/local/ORS_workbook.xlsx"
sheet_name = "HIV2015"
```

Run all cells. The notebook will export **`impact_score.csv`**.

---

## Model Overview (What is being computed)

For each **country** and each **drug** present in the HIV 2015 ORS sheet:

1. Treatment is evaluated in four components:
   - Adult first-line
   - Adult second-line
   - Child first-line
   - Child second-line

2. Regimen tables are scanned. If a regimen contains a given drug, its contribution is added using the ORS HIV model terms (DALYs, coverage, regimen share, efficacy, and the model adjustment term).

3. The total is normalized using the retention-rate adjustment described in the ORS supporting documentation (effective treatment duration adjustment).

This produces an Impact Score for every **Country × Drug** combination, exported to `impact_score.csv`.

---

## Implementation Notes

- The ORS sheet is loaded as a **raw Excel grid** (`header=None`) to preserve the original ORS layout and multi-row headers.
- The implementation is **header-anchored (entity-map style)** so inputs are referenced via anchored labels/positions rather than fragile hard-coded indices.
- Percentage-like fields are normalized to fractional form when required.

---

## Validation

The notebook includes validation by directly comparing the computed per-country, per-drug values against the corresponding Excel outputs.

Maximum absolute differences are on the order of **~1e-6**, consistent with floating-point precision.

---

## Note on `.gitignore`

`.gitignore` only affects what Git tracks by default on your local machine. It helps prevent accidentally committing:
- local virtual environments (e.g., `.venv/`)
- Python caches (`__pycache__/`, `*.pyc`)
- notebook checkpoints (`.ipynb_checkpoints/`)
- local data copies (e.g., a `data/` folder)

If `impact_score.csv` is intentionally included in the repo (as required by this task), it should **not** be ignored in `.gitignore`.
