<!-- Portfolio banner -->
<p align="center">
  <img src="docs/assets/banner.png" alt="Power BI Portfolio Banner" width="100%" onerror="this.style.display='none'"/>
</p>

<h1 align="center">Power BI Data &amp; Reporting Portfolio</h1>

<p align="center">
  <em>Synthetic, end-to-end BI projects spanning Banking, Construction, Energy and Healthcare — from raw dataset to wireframe to production-style Power BI dashboard.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" alt="Power BI"/>
  <img src="https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" alt="DAX"/>
  <img src="https://img.shields.io/badge/Power%20Query-FCBA03?style=for-the-badge&logo=powerbi&logoColor=black" alt="Power Query"/>
  <img src="https://img.shields.io/badge/Data%20Modeling-0F4C81?style=for-the-badge" alt="Data Modeling"/>
  <img src="https://img.shields.io/badge/Synthetic%20Data-2E7D32?style=for-the-badge" alt="Synthetic Data"/>
  <img src="https://img.shields.io/badge/Public%20Sector%20Friendly-512BD4?style=for-the-badge" alt="Public Sector"/>
</p>

<p align="center">
  <a href="#-featured-dashboards">Featured</a> ·
  <a href="#-project-index">Projects</a> ·
  <a href="#-repository-structure">Structure</a> ·
  <a href="#-how-to-explore">How to explore</a> ·
  <a href="#-conventions--standards">Standards</a> ·
  <a href="#-about-the-author">Author</a>
</p>

---

## Why this repo exists

This portfolio demonstrates how I take a business problem from a written brief, through a wireframe, to a finished Power BI report — using the same discipline expected in regulated and public-sector environments. Every dataset here is fully synthetic and was generated specifically to exercise BI patterns (slowly changing dimensions, time-intelligence, regulatory reporting, real-time signal detection, forecasting). Nothing in this repository contains real customer, patient, transaction, or grid-operator data.

Whether you're a recruiter, a public-sector reviewer, or a fellow analyst, the goal is the same: **walk into any project folder and understand the business problem, the data, the design choices, and the final dashboard inside two minutes.**

---

## Featured dashboards

> Replace the preview images below with screenshots exported from Power BI Desktop (`File → Export → PowerPoint` or `File → Export → PDF`, then crop a hero image per page). Save each preview to the project's `reports/exports/screenshots/` folder and update the path here.

<table>
<tr>
<td width="33%" align="center">
  <a href="projects/banking-and-fintech/01-fraud-detection/">
    <img src="projects/banking-and-fintech/01-fraud-detection/reports/exports/screenshots/preview.png" alt="Fraud Detection preview" width="100%" onerror="this.src='docs/assets/placeholder-fraud.png'"/>
    <br/><strong>Real-Time Fraud Detection</strong>
  </a>
  <br/><sub>Banking &amp; Fintech · transaction-level signal scoring</sub>
</td>
<td width="33%" align="center">
  <a href="projects/construction-and-buildings/01-bjf-group-revenue-operations/">
    <img src="projects/construction-and-buildings/01-bjf-group-revenue-operations/reports/exports/screenshots/preview.png" alt="BJF Revenue preview" width="100%" onerror="this.src='docs/assets/placeholder-bjf.png'"/>
    <br/><strong>BJF Group Revenue &amp; Operations</strong>
  </a>
  <br/><sub>Construction · multi-project revenue, margin and progress</sub>
</td>
<td width="33%" align="center">
  <a href="projects/healthcare-and-life-sciences/01-patient-360-clinical-trials/">
    <img src="projects/healthcare-and-life-sciences/01-patient-360-clinical-trials/reports/exports/screenshots/preview.png" alt="Patient 360 preview" width="100%" onerror="this.src='docs/assets/placeholder-patient.png'"/>
    <br/><strong>Patient 360 &amp; Clinical Trial Funnel</strong>
  </a>
  <br/><sub>Healthcare · enrolment, AE safety, site velocity</sub>
</td>
</tr>
</table>

---

## Project index

| # | Domain | Project | Business question | Status |
|---|--------|---------|-------------------|--------|
| 1 | Banking &amp; Fintech | [Real-Time Fraud Detection](projects/banking-and-fintech/01-fraud-detection/) | Which transactions look fraudulent in flight, and what's our false-positive cost? | v1 |
| 2 | Banking &amp; Fintech | [Liquidity &amp; Settlement Risk](projects/banking-and-fintech/02-liquidity-settlement-risk/) | Are we within liquidity coverage and intraday settlement tolerances? | v1 |
| 3 | Banking &amp; Fintech | [Regulatory Suspicious Activity (SAR)](projects/banking-and-fintech/03-regulatory-suspicious-activity/) | Where are SAR backlogs and which typologies are emerging? | v1 |
| 4 | Construction &amp; Buildings | [BJF Group Revenue &amp; Operations](projects/construction-and-buildings/01-bjf-group-revenue-operations/) | How is each construction project performing on revenue, margin and schedule? | v1 |
| 5 | Energy &amp; Utilities | [Grid Load Forecasting &amp; DER Impact](projects/energy-and-utilities/01-grid-load-forecasting-der/) · [🌐 live wireframe](https://htmlpreview.github.io/?https://github.com/roscar747/data-reporting/blob/main/projects/energy-and-utilities/01-grid-load-forecasting-der/wireframes/low-fidelity/grid_load_forecast_der_dashboard_wireframe_v1.html) | Where will distributed energy resources stress the grid next? | v1 |
| 6 | Healthcare &amp; Life Sciences | [Patient 360 &amp; Clinical Trial Funnel](projects/healthcare-and-life-sciences/01-patient-360-clinical-trials/) | Which trial sites convert patients fastest and where are AE risks concentrating? | v1 |

Every project page contains: business problem, dataset description, KPI definitions, wireframe-vs-final comparison, screenshots, and a link to the `.pbix`.

---

## Repository structure

```text
data-reporting/
├── README.md                         ← you are here (portfolio-level)
├── .gitattributes                    ← Git LFS rules for .pbix / .pbit / large CSV
├── .gitignore                        ← excludes Power BI cache + local settings
│
├── docs/                             ← repo-wide standards & guides
│   ├── naming-conventions.md
│   ├── publishing-checklist.md
│   ├── powerbi-best-practices.md
│   └── assets/                       ← banner, badges, placeholder images
│
├── templates/                        ← copy these when starting a new project
│   ├── project-readme-template.md
│   ├── data-dictionary-template.md
│   └── kpi-definitions-template.md
│
└── projects/                         ← one folder per business domain
    ├── banking-and-fintech/
    │   ├── 01-fraud-detection/
    │   ├── 02-liquidity-settlement-risk/
    │   └── 03-regulatory-suspicious-activity/
    ├── construction-and-buildings/
    │   └── 01-bjf-group-revenue-operations/
    ├── energy-and-utilities/
    │   └── 01-grid-load-forecasting-der/
    └── healthcare-and-life-sciences/
        └── 01-patient-360-clinical-trials/
```

Every project folder follows the same internal layout, so once you've navigated one project you can navigate any of them:

```text
projects/<domain>/<NN>-<project-name>/
├── README.md                         ← project story (problem → data → KPIs → dashboard)
├── data/
│   ├── raw/                          ← source CSVs as generated / received
│   └── processed/                    ← cleaned / shaped tables consumed by Power BI
├── reports/
│   ├── pbix/                         ← packaged Power BI files (Git LFS)
│   ├── pbip/                         ← Power BI Project format (when used) – diff-friendly
│   └── exports/
│       ├── pdf/                      ← full-report PDF exports
│       └── screenshots/              ← per-page PNGs and the README hero image
├── wireframes/
│   ├── low-fidelity/                 ← early sketches / HTML mocks
│   └── high-fidelity/                ← polished pre-build mocks aligned to PBI visuals
└── docs/
    ├── data-dictionary.md            ← field-level documentation for every table
    ├── kpi-definitions.md            ← every KPI / measure with its DAX intent
    └── source/                       ← original briefs (.docx) and supporting material
```

---

## How to explore

**If you have 60 seconds.** Open the project index above, click into the project that matches the domain you care about, and read the *Business problem* and *Key insights* sections of its README. Each one has a hero screenshot.

**If you have 5 minutes.** Pick a project, scroll through its README in order, then open the PDF in `reports/exports/pdf/` for a static walkthrough of every page.

**If you want to open the actual report.** Clone the repo (with Git LFS installed — see [Power BI best practices](docs/powerbi-best-practices.md)) and open the `.pbix` file in Power BI Desktop. The `.pbip` variant of the BJF project lets you read the model and report definitions as plain text — useful for code review.

```bash
# One-time setup (only needed once per machine)
git lfs install

# Clone the repo with .pbix files materialized
git clone https://github.com/{{GITHUB_USER}}/data-reporting.git
cd data-reporting
git lfs pull
```

---

## Conventions &amp; standards

This repo treats documentation as a first-class deliverable. The standards below are enforced by convention, not tooling, but they make the repo predictable for anyone landing on it cold.

- **Naming conventions** — folders, Power BI files, datasets, wireframes and screenshots all follow the rules in [`docs/naming-conventions.md`](docs/naming-conventions.md). Business-friendly names on the outside, snake_case under the hood.
- **Publishing checklist** — before any project goes public, it passes the gates in [`docs/publishing-checklist.md`](docs/publishing-checklist.md): synthetic-data confirmation, screenshots present, KPI definitions complete, no embedded credentials.
- **Power BI best practices** — file size, versioning, Git LFS, public links, screenshot capture and PDF export are all covered in [`docs/powerbi-best-practices.md`](docs/powerbi-best-practices.md).
- **Templates** — every new project starts by copying the templates in [`templates/`](templates/). The per-project READMEs in this repo are themselves filled-in copies of that template.

---

## Data &amp; privacy

Every dataset in this repository is **fully synthetic** and was generated for the purpose of exercising BI and ML patterns. There are no real customers, patients, transactions, settlements, grid operators or construction projects represented here. CSV row counts are deliberately modest (low single-digit MB) so the repo stays clone-friendly without LFS for data; Git LFS is reserved for the `.pbix` / `.pbit` artifacts. Each project README repeats this declaration in its own *Data &amp; privacy* section.

If you are evaluating this portfolio on behalf of a public-sector or regulated organisation and need a written confirmation of the synthetic-data status, the [`docs/publishing-checklist.md`](docs/publishing-checklist.md) gate-list documents the controls applied before publication.

---

## About the author

**{{YOUR_NAME}}** — Lead BI Engineer. I design Power BI solutions end-to-end: requirements, semantic modelling, DAX, visual design, performance tuning, and the documentation that lets the next engineer pick up where I left off.

- LinkedIn: {{LINKEDIN_URL}}
- GitHub: [@{{GITHUB_USER}}](https://github.com/{{GITHUB_USER}})
- Headline: {{HEADLINE}}
- Location: {{LOCATION}}

---

<sub>Last updated: 2026-04-28 · License: see <a href="LICENSE">LICENSE</a> (add a license file if publishing publicly — MIT or CC-BY-4.0 are common choices for portfolio repos).</sub>
