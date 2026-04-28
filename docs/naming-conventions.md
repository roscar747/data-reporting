# Naming conventions

The portfolio follows a single rule: **what the user sees should read like English; what Git tracks should sort and diff cleanly.** Folder and file names are picked accordingly.

## Folders

| Level | Style | Example |
|------|-------|---------|
| Domain | `lower-kebab-case` | `banking-and-fintech/` |
| Project | `NN-lower-kebab-case` | `01-fraud-detection/` |
| Subfolders | fixed vocabulary, `lower-kebab-case` | `data/`, `reports/`, `wireframes/`, `docs/` |

The `NN-` prefix on projects controls the order projects appear in GitHub's web view. Use `01`, `02`, `03` — leave room to insert. Don't renumber retroactively; if a project is retired, leave its number as a tombstone (`05-retired-old-finance/`) or move the folder to `archive/`.

## Power BI files (`.pbix`, `.pbit`, `.pbip`)

| Pattern | Example |
|---------|---------|
| `<BusinessName>_v<major>.pbix` | `Fraud_Detection_v1.pbix` |
| `<BusinessName>_v<major>.pbit` | `BJF_Group_Revenue_Operation_Report_v1.pbit` |
| `<BusinessName>.pbip` | `BJF_Group_Revenue_Operation_Report.pbip` |

Rules:

1. Use `Title_Case_With_Underscores` for the report stem. Power BI Desktop displays this name in browser tabs and recent-files lists, so it should read like a deliverable title.
2. Append `_v1`, `_v2`, etc. only on `.pbix` / `.pbit`. Power BI Project (`.pbip`) is the diff-friendly source format, so version it through Git, not the filename.
3. Avoid spaces. They are legal in Power BI but they break shell commands, breadcrumbs, and CI.
4. Avoid `final`, `FINAL_v2`, `final_actually_final`. Numeric versions only.

## Datasets (`.csv`, `.parquet`)

| Pattern | Example |
|---------|---------|
| `<subject>_<scope>_<startYear>_<endYear>.csv` | `synthetic_fraud_detection_data_2023_2026.csv` |
| `<subject>_<scope>.parquet` | `liquidity_settlement_risk_dataset.parquet` |

Rules:

1. `lower_snake_case` everywhere — datasets are read by code more often than by humans.
2. The year span at the end is optional but recommended for time-series data; it answers "is this stale?" at a glance.
3. Use `synthetic_`, `mock_`, or `dummy_` prefix for any fabricated data destined for a public repo. The prefix doubles as a privacy declaration.

## Wireframes (`.html`, `.png`, `.fig`)

| Pattern | Example |
|---------|---------|
| `<report>_<view>_wireframe_v<n>.html` | `fraud_detection_dashboard_wireframe_v1.html` |
| `<report>_<view>_wireframe_v<n>.png` | `liquidity_overview_wireframe_v2.png` |
| Final-render mock | `<report>_<view>_dashboard_v<n>.html` |

Rules:

1. The word `wireframe` indicates a non-final design artifact.
2. The word `dashboard` (without `wireframe`) indicates a high-fidelity mock that should look 1:1 with the final Power BI page.
3. Versioning is per-file, so a redesign produces `_v2` rather than overwriting `_v1`. Keep both — the design evolution is part of the story.

## Screenshots (`reports/exports/screenshots/`)

| Pattern | Example |
|---------|---------|
| Hero image | `preview.png` |
| Per-page | `01-overview.png`, `02-trends.png`, `03-detail.png` |
| Animated walkthrough | `walkthrough.gif` |

The page screenshots are numbered to match the report's left-nav order. Recruiters frequently look at GIFs first — keep them under 5 MB and under 15 seconds.

## PDF exports

| Pattern | Example |
|---------|---------|
| `<BusinessName>_v<n>.pdf` | `Fraud_Detection_v1.pdf` |

One PDF per `.pbix` version. Old PDFs stay in the folder as a paper trail.

## Documentation files inside a project

| File | Required? | Purpose |
|------|-----------|---------|
| `README.md` | Yes | The project story. Copied from `templates/project-readme-template.md`. |
| `docs/data-dictionary.md` | Yes | Field-level documentation for every table consumed by the report. |
| `docs/kpi-definitions.md` | Yes | Every KPI / measure with its DAX intent and business meaning. |
| `docs/source/` | Optional | Original briefs, regulatory notes, research material. |

## Worked examples — applying the conventions to this repo

Below is the full mapping from the legacy names that existed before reorganization to the names this repo now uses. Use it as a cheat sheet when you encounter old links.

| Old name | New name |
|----------|----------|
| `Banking_and_Fintech/Dummy Dataset - ML Testing B/Fraud_Detection.pbix` | `projects/banking-and-fintech/01-fraud-detection/reports/pbix/Fraud_Detection_v1.pbix` |
| `Banking_and_Fintech/Dummy Dataset - ML Testing B/synthetic_fraud_detection_data_2023_2026.csv` | `projects/banking-and-fintech/01-fraud-detection/data/raw/synthetic_fraud_detection_data_2023_2026.csv` |
| `Banking_and_Fintech/Dummy Dataset - ML Testing B/fraud_detection_pbi_dashboard_wireframe.html` | `projects/banking-and-fintech/01-fraud-detection/wireframes/low-fidelity/fraud_detection_dashboard_wireframe_v1.html` |
| `Banking_and_Fintech/Dummy Dataset - ML Testing B/real_time_fraud_signals_dashboard.html` | `projects/banking-and-fintech/01-fraud-detection/wireframes/high-fidelity/real_time_fraud_signals_dashboard_v1.html` |
| `Construction_and_Buildings/BJF_Group_Revenue_Operation_Report.pbix` | `projects/construction-and-buildings/01-bjf-group-revenue-operations/reports/pbix/BJF_Group_Revenue_Operation_Report_v1.pbix` |
| `Construction_and_Buildings/BJF_Group_Revenue_Operation_Report.pbip` (+ `.Report` and `.SemanticModel` folders) | `projects/construction-and-buildings/01-bjf-group-revenue-operations/reports/pbip/...` |
| `Energy_and_Utilities/Dummy Dataset - ML Testing E/Grid_Load_Forecasting_and_DER_Impact_Analysis.pbix` | `projects/energy-and-utilities/01-grid-load-forecasting-der/reports/pbix/Grid_Load_Forecasting_and_DER_Impact_Analysis_v1.pbix` |
| `Healthcare_and_Life_Sciences/Dummy Dataset - ML Testing M/Patient_360 and_Clinical_Trial_Operational_Funnel.pbix` | `projects/healthcare-and-life-sciences/01-patient-360-clinical-trials/reports/pbix/Patient_360_and_Clinical_Trial_Operational_Funnel_v1.pbix` |
