# HDB Resale Flat Price ETL - Part 1

This repository contains an executable ETL pipeline (notebook) that processes HDB resale flat price CSVs and produces cleaned, transformed, failed and hashed datasets for January 2012–December 2016.

## What is included

- Executable Jupyter notebook: `part1/HDB_ETL_Pipeline.ipynb`
- Input CSVs: `part1/raw/` (5 source CSV files)
- Outputs: `part1/output/` (raw, cleaned, transformed, failed, hashed)
- Architecture design: `part2/architecture/hdb_architecture.png`
- Minimal dependency list: `requirements.txt`

## Output groups (locations)

- Raw: `part1/output/raw/master_raw_data.csv` — combined source data
- Cleaned: `part1/output/cleaned/cleaned_data.csv` — records that passed validation and deduplication
- Transformed: `part1/output/transformed/transformed_data.csv` — cleaned records + `resale_identifier`
- Failed: `part1/output/failed/failed_data.csv` — rejected records with `failure_reason`
- Hashed: `part1/output/hashed/hashed_data_anonymized.csv` — hashed identifiers

## How to run

1. Install dependencies (optional if already installed):

```
pip install -r requirements.txt
```

2. Run the notebook (interactive):

```
jupyter notebook part1/HDB_ETL_Pipeline.ipynb
```

Or run non-interactively (reproduces the executed outputs):

```
jupyter nbconvert --to notebook --execute part1/HDB_ETL_Pipeline.ipynb --ExecutePreprocessor.timeout=3600
```

3. Check generated outputs in `part1/output/`.

## Notes & assumptions

- The pipeline uses the CSV files staged in `part1/raw/`; no additional copying is required.
- Validation rules and thresholds are implemented in the notebook (date, town, flat_type, flat_model, storey_range, floor_area, price).
- Remaining lease is recomputed from `lease_commence_date` as of the run date (99-year lease assumption) and formatted as `"XX years YY months"`.
- Duplicate handling: composite key = all fields except `resale_price`; the highest-price record is retained, lower-priced duplicates moved to the failed dataset with `failure_reason`.
- Anomaly detection: IQR and z-score (and price-per-sqm) heuristics applied per `town` + `flat_type` groups.
- Resale identifiers are hashed using SHA-256; anonymized version excludes the original identifier.



