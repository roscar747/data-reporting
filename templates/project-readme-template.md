<!--
  PROJECT README TEMPLATE
  ------------------------
  Copy this file to projects/<domain>/<NN>-<project-name>/README.md and fill in
  every {{placeholder}}. Delete sections that genuinely don't apply, but err on
  the side of keeping them — completeness is part of what makes the portfolio
  feel professional.
-->

# {{Project Title}}

<p>
  <img src="https://img.shields.io/badge/Domain-{{Domain}}-0F4C81?style=flat-square" alt="Domain"/>
  <img src="https://img.shields.io/badge/Status-v1-2E7D32?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/Data-Synthetic-F2C811?style=flat-square&labelColor=333" alt="Synthetic data"/>
  <img src="https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black" alt="Power BI"/>
</p>

> **One-line pitch.** {{The problem solved, in plain English, in under 25 words.}}

<p align="center">
  <img src="reports/exports/screenshots/preview.png" alt="{{Project Title}} hero screenshot" width="100%"/>
</p>

---

## Business problem

{{Two to four short paragraphs describing the situation that motivates this dashboard. Write it for a stakeholder who has never met you. Avoid acronyms; if you must use one, define it. Examples of what to cover: who the users are, what decision they need to make, what they currently use instead, and why that's not enough.}}

## Dataset description

| Table | Grain | Rows | File |
|-------|-------|------|------|
| `{{table_name}}` | {{one row per … }} | {{~N}} | [`data/raw/{{filename}}.csv`](data/raw/{{filename}}.csv) |

**Source.** {{Where the data came from. For this portfolio, every table is synthetic — describe how it was generated and what real-world distribution it tries to mimic.}}

**Time range covered.** {{e.g. 2023-01-01 through 2026-03-31}}

**Refresh cadence.** {{Static / Daily / Hourly / Streaming — and how that's modelled in Power BI.}}

Field-level documentation: [`docs/data-dictionary.md`](docs/data-dictionary.md).

## Key KPIs and insights

| KPI | What it answers | Target / threshold | Where it appears |
|-----|------------------|-------|------------------|
| {{KPI name}} | {{Question this KPI answers}} | {{e.g. > 95%}} | {{Page name}} |

Full DAX definitions and business meaning: [`docs/kpi-definitions.md`](docs/kpi-definitions.md).

**Headline insights.**

1. {{Insight one — observed pattern, stated as a finding rather than a feature.}}
2. {{Insight two.}}
3. {{Insight three — ideally something a stakeholder would act on.}}

## Wireframe vs final dashboard

The design moved through low-fidelity HTML wireframes, a high-fidelity mock aligned to native Power BI visuals, and the final report. Each step is preserved so the design evolution is reviewable.

| Stage | Preview | Source |
|-------|---------|--------|
| Low-fidelity wireframe | <img src="wireframes/low-fidelity/{{wireframe_image}}.png" width="320" alt="Low-fidelity wireframe"/> | [`wireframes/low-fidelity/`](wireframes/low-fidelity/) |
| High-fidelity mock | <img src="wireframes/high-fidelity/{{hifi_image}}.png" width="320" alt="High-fidelity mock"/> | [`wireframes/high-fidelity/`](wireframes/high-fidelity/) |
| Final Power BI report | <img src="reports/exports/screenshots/01-overview.png" width="320" alt="Final Power BI overview"/> | [`reports/pbix/`](reports/pbix/) |

**What changed between wireframe and final.** {{Two to four bullets describing design decisions made along the way: which visuals were swapped, what was added during user feedback, what was removed for clarity.}}

## Report pages

| # | Page | What it shows |
|---|------|---------------|
| 1 | {{Overview}} | {{Headline KPIs and the page that loads first.}} |
| 2 | {{Trends}} | {{Time-series breakdowns.}} |
| 3 | {{Detail}} | {{Drill-through / row-level detail.}} |

<table>
<tr>
<td><img src="reports/exports/screenshots/01-overview.png" width="320" alt="Page 1"/></td>
<td><img src="reports/exports/screenshots/02-trends.png" width="320" alt="Page 2"/></td>
<td><img src="reports/exports/screenshots/03-detail.png" width="320" alt="Page 3"/></td>
</tr>
</table>

## Open the report

- 📊 **Power BI file** — [`reports/pbix/{{filename}}.pbix`](reports/pbix/{{filename}}.pbix) *(Git LFS, ~{{size}} MB)*
- 📄 **PDF export** — [`reports/exports/pdf/{{filename}}.pdf`](reports/exports/pdf/{{filename}}.pdf)
- 🌐 **Live link** *(if published)* — {{https://app.powerbi.com/view?r=...}}

To open the `.pbix` you'll need [Power BI Desktop](https://www.microsoft.com/power-platform/products/power-bi/desktop) and Git LFS — see [`docs/powerbi-best-practices.md`](../../../docs/powerbi-best-practices.md).

## Data &amp; privacy

All data in this project is **synthetic**. The CSVs were generated to exercise BI patterns and do not represent real customers, patients, transactions, or any other real-world entities. There is no PII, PCI, PHI or other regulated content in the repository.

## Tech notes

- **Semantic model.** {{Star schema with N fact tables and M dimensions. Note any role-playing dimensions or calculation groups.}}
- **DAX patterns of interest.** {{e.g. SAMEPERIODLASTYEAR for YoY, calculation groups for time-intelligence, dynamic measure selection.}}
- **Power Query.** {{Brief description of the cleansing / shaping steps.}}
- **Performance.** {{Slowest visual on cold open, what was tuned.}}

## Related

- 📁 Domain index: [`projects/{{domain}}/`](../)
- 📚 Repo standards: [naming](../../../docs/naming-conventions.md) · [Power BI](../../../docs/powerbi-best-practices.md) · [publishing checklist](../../../docs/publishing-checklist.md)
