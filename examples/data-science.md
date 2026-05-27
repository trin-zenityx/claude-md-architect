# Example: Data Science / Research Project

Shows how CLAUDE.md works for non-software-engineering projects. Domain-specific gotchas are the highest-value content here.

## The File

```markdown
# Customer Churn Analysis
Predict churn for telecom customers using historical usage data. Output:
ranked list of at-risk customers for retention team.

Stack: Python 3.11, pandas 2.2, scikit-learn 1.4, Jupyter, plotly.

## Data
- `data/raw/` — immutable raw exports from BigQuery; **never modify**
- `data/processed/` — cleaned, ready for modeling
- `data/intermediate/` — exploratory transforms
- `data/external/` — third-party demographic data

## Commands
- `jupyter lab` — start notebook server
- `python -m src.pipeline.run` — full preprocessing pipeline
- `python -m src.experiments.train --config configs/baseline.yaml` — train model
- `python -m src.reports.generate` — render final report

## Structure
- `notebooks/` — exploratory work, numbered (01_eda.ipynb, 02_features.ipynb, ...)
- `src/` — reusable code (notebooks import from src, NOT the reverse)
- `experiments/` — training runs with logged params and metrics (MLflow)
- `reports/` — deliverables for the retention team

## Conventions
- Use TimeSeriesSplit for cross-validation, NOT random KFold (data has temporal leakage)
- Target variable is always named `churn` (0/1 integer, not "yes"/"no" string)
- Set random_state=42 in every experiment for reproducibility
- Document model choice in `analysis_log.md` with rationale AND metrics
- Notebooks are exploratory; production logic must move to `src/` with tests

## Rules
- Never modify files in `data/raw/`
- Never commit data files (.gitignore covers data/, DVC handles versioning)
- Pin dependencies in `requirements.txt`
- Use `logging`, not `print()`, for long-running pipelines
- **IMPORTANT:** Set random seeds explicitly; do not rely on defaults

## Gotchas
- Customer IDs are strings with leading zeros ("00123"), NOT integers — preserve as str
- `subscription_end_date` is null for active customers; handle as "no end date", not missing
- Don't use accuracy metric — dataset is 90:10 imbalanced; use F1 or AUROC
- BigQuery timestamps are in UTC; customer-facing reports need conversion to PT
- The "churn" definition changed in Q3 2025; do not mix pre/post-Q3 data without flagging

## See also
- Data dictionary: `docs/data-dictionary.md`
- Reproducibility notes: `docs/reproducibility.md`
- Past experiments and learnings: `analysis_log.md`
```

**Total: ~50 lines.**

## Annotations

**Identity (lines 1-5)**: Mentions the *deliverable* (ranked list for retention team), not just the *technical task* (build a model). This tells Claude what success looks like, not just what to build.

**Data section (7-12)**: Critical for research projects. The "**never modify**" emphasis on raw data is a hard rule worth bolding. Folder purposes prevent Claude from putting processed data in raw/, etc.

**Commands (14-18)**: Project uses script-based commands (not just notebooks). Each command has a purpose. The `--config configs/baseline.yaml` pattern shows Claude how to invoke configurable runs.

**Conventions (24-30)**: Six conventions, all specific to data science and this domain:
- TimeSeriesSplit vs KFold — domain-specific (temporal data)
- Naming convention for target — prevents subtle bugs
- Random seed practice — reproducibility
- Documentation requirement — knowledge accumulation
- Notebooks vs src separation — research vs production code

**Gotchas (38-44)**: This is where this template shines. Each gotcha is a real domain trap:
- Customer ID as string (a common ML pipeline footgun)
- Null end_date semantics (data-specific, easy to misinterpret)
- Imbalanced dataset → wrong metric (common ML mistake)
- Timezone awareness (common reporting issue)
- Definition change in Q3 (institutional knowledge — invaluable)

These are NOT inferable from the data. Without this section, Claude would make these mistakes.

## What's notably absent

- ❌ "Use type hints" — Python style, linter or just Python expectation
- ❌ "Write tests for src/" — implied by professionalism; mention if not happening
- ❌ "Follow PEP 8" — Black/Ruff handle it
- ❌ Generic data science best practices ("split train/test before EDA") — Claude knows

## What this enables

A new session opening this project can immediately:
- Know what the deliverable is
- Avoid modifying raw data
- Use the right CV strategy
- Avoid the accuracy trap
- Handle dates and IDs correctly
- Know where to look for past decisions (analysis_log.md)

Without this CLAUDE.md, Claude would likely:
- Use random KFold (subtle bug, hard to spot)
- Report accuracy proudly (~90% — wrong metric on imbalanced data)
- Cast customer IDs to int (data corruption)
- Mix pre/post-Q3 data (silently wrong analysis)

## Lessons for other research projects

1. **Domain gotchas are 80% of value** — invest most lines here
2. **Tell Claude what success looks like** — the deliverable, not just the task
3. **Be explicit about data rules** — never modify raw, naming conventions
4. **Reproducibility is non-negotiable** — random seeds, pinned versions, logged experiments
5. **Point to evolving knowledge** — analysis_log.md grows; CLAUDE.md doesn't need to
