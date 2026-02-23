# GHI Data Team Skills Task — HIV 2015 Impact Score

This repository contains a single Jupyter notebook that reproduces the HIV 2015 ORS Impact Score calculation in Python and exports the required `impact_score.csv`.

The workflow follows the structure outlined in the HIV 2013 reference notebook and the Entity Map guide, and implements the formula described in the ORS supporting documentation.

---

## File Included

`HIV_2015_impact_score.ipynb`

This notebook performs the complete workflow:

- Loads the HIV 2015 ORS sheet as a raw Excel grid  
- Constructs a header-anchored entity map  
- Extracts all required country-level inputs  
- Computes Impact Score for each country × each drug  
- Validates computed values against the Excel sheet  
- Exports `impact_score.csv`  

---

## How to Run

1. Make a local copy of the ORS workbook (as instructed in the assignment).  
2. Open `HIV_2015_impact_score.ipynb`.  
3. Set the following variables inside the notebook:

```python
xlsx_path = "path/to/your/local/ORS_file.xlsx"
sheet_name = "HIV2015"
```

4. Run all cells.  
5. The notebook will generate `impact_score.csv`.

---

## Model Overview

For each country and each drug present in the HIV 2015 ORS sheet:

1. Treatment is divided into four components:
   - Adult first-line
   - Adult second-line
   - Child first-line
   - Child second-line

2. For each regimen containing a given drug:
   - The contribution to DALYs averted is computed using the ORS model formula.
   - Coverage, regimen share, efficacy, and DALYs are incorporated.
   - The model adjustment term is applied.

3. The total is normalized using the retention-rate adjustment described in the ORS supporting documentation.

This produces an Impact Score for every Country × Drug combination.

---

## Implementation Notes

- The sheet is loaded using `header=None` to preserve the original ORS layout.
- The implementation is header-anchored (entity-map style), so all inputs are referenced using stable column labels or anchored cell positions rather than fragile positional assumptions.
- Percentage-like values are normalized to fractional form where required.

---

## Validation

Computed Impact Scores were validated against the Excel ORS sheet by comparing per-country and per-drug outputs.

Maximum absolute differences are on the order of ~1e-6, reflecting floating-point precision only.
