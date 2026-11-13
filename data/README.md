# data/

Datasets used by the project.

- `raw/` : original data, never edited by hand.
- `processed/` : cleaned data produced by scripts in `src/data/`.

Rules:
- Follow the schema in [`../docs/DATA_SCHEMA.md`](../docs/DATA_SCHEMA.md).
- **Anonymize student data** (no real names or national IDs).
- If real academic data is not available, generate synthetic data and say so clearly in the report.
- Do not commit large files; keep small samples and document where the full data comes from.
