# [Project Name]
[One-sentence: what is this research project investigating?]

Stack: [Python version], [pandas/polars version], [scikit-learn/PyTorch version], [Jupyter/notebook tool].

## Project Overview
- **Research question**: [the question this project answers]
- **Goal**: [the actionable outcome]
- **Stakeholders**: [who uses the results]

## Data
- `data/raw/` — immutable raw exports; **never modify**
- `data/processed/` — cleaned, ready for modeling
- `data/intermediate/` — exploratory transforms
- `data/external/` — third-party data sources

## Commands
- `[command to run notebooks]` — start Jupyter
- `[command to run pipeline]` — run full preprocessing
- `[command to run experiment]` — run training/analysis
- `[command to generate report]` — create output document

## Structure
- `notebooks/` — exploratory work (numbered: `01_eda.ipynb`, `02_features.ipynb`, ...)
- `src/` — reusable Python modules (import from notebooks, not the reverse)
- `experiments/` — model training runs with logged params and metrics
- `reports/` — final deliverables

## Conventions
- [Cross-validation strategy: "Use TimeSeriesSplit, not random KFold"]
- [Target variable naming: "Always named `target`, binary 0/1"]
- [Random seed: "Set seed=42 in all experiments for reproducibility"]
- Document model decisions in `analysis_log.md` with rationale and metrics
- Notebooks are exploratory — production code lives in `src/`, with tests

## Rules
- Never modify `data/raw/`
- Never commit data files (use .gitignore + DVC or similar)
- Pin all dependencies in `requirements.txt` or `pyproject.toml`
- Use [logger / tqdm], not print statements, for long-running scripts
- **IMPORTANT:** Set random seeds explicitly in every experiment

## Gotchas
- [Domain-specific data quirk: "Customer IDs are strings with leading zeros"]
- [Edge case: "subscription_end_date is null for active customers"]
- [Trap: "Don't use accuracy on this imbalanced dataset; use F1 or AUROC"]

## See also
- Decisions log: `analysis_log.md`
- Data dictionary: `docs/data-dictionary.md`
- Reproducibility notes: `docs/reproducibility.md`
