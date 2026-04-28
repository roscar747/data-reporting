# Regulatory Suspicious Activity (SAR)

<p>
  <img src="https://img.shields.io/badge/Domain-Banking%20%26%20Fintech-0F4C81?style=flat-square" alt="Domain"/>
  <img src="https://img.shields.io/badge/Status-v1-2E7D32?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/Data-Synthetic-F2C811?style=flat-square&labelColor=333" alt="Synthetic data"/>
  <img src="https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black" alt="Power BI"/>
</p>

> **One-line pitch.** Make Suspicious Activity Report (SAR) backlog, typology and disposition visible to compliance leads in one place — what regulators ask for, before they ask.

<p align="center">
  <img src="reports/exports/screenshots/preview.png" alt="SAR Compliance hero screenshot" width="100%"/>
</p>

---

## Business problem

Financial-crime teams file Suspicious Activity Reports (SARs) to regulators on a tight clock. The challenge is rarely a single report — it's the portfolio: how many SARs are open, where are they aging, which typologies are emerging this quarter, and which reporting institution / agency pair is generating the most rework.

This dashboard demonstrates how a Power BI report can serve both the compliance lead (who needs the daily backlog view) and the head of financial crime (who needs the quarterly trend and typology mix). It is designed so the same report is the answer to a regulator's "show me what you saw and when" question.

## Dataset description

| Table | Grain | Rows | File |
|-------|-------|------|------|
| `regulatory_sar_stubs_2023_2026` | One row per filed SAR | ~5,000 | [`data/raw/regulatory_sar_stubs_2023_2026.csv`](data/raw/regulatory_sar_stubs_2023_2026.csv) |

Schema: `sar_id`, `filing_date`, `regulatory_agency`, `reporting_institution`, `subject_type`, `subject_occupation_industry`, `activity_start_date`, `activity_end_date`, `total_amount_involved`, `currency`, `suspicion_category`, `is_urgent_filing`, `disposition_status`, `narrative_summary_stub`, `associated_account_count`.

**Source.** Synthetic. SAR narratives are stubs (placeholder strings) — never real text. Subject types, industries and suspicion categories follow the public taxonomies used in major jurisdictions (FinCEN-style buckets), without any real subjects.

**Time range covered.** 2023-01-01 through 2026-03-31.

Full field definitions: [`docs/data-dictionary.md`](docs/data-dictionary.md).

## Key KPIs and insights

| KPI | What it answers | Target / threshold | Where it appears |
|-----|------------------|-------|------------------|
| Open SARs | How many SARs are awaiting disposition? | Watch | Overview |
| Average days to disposition | How long does an SAR take to close? | ≤ 30 days | Overview, Trends |
| Urgent-filing share | What % of SARs were filed under urgent rules? | Watch | Overview |
| SARs by typology | Which suspicion categories are growing? | n/a | Trends |
| Agency mix | Which regulators are receiving the most filings? | n/a | Trends |

Full DAX: [`docs/kpi-definitions.md`](docs/kpi-definitions.md).

**Headline insights.**

1. The urgent-filing share has trended up over the synthetic 2023–2026 window — the dashboard makes it obvious where the spike came from (a single typology in a single quarter).
2. Average days to disposition varies by reporting institution by a factor of three; the institution-level slicer surfaces who is faster and gives the head of financial crime an obvious place to ask why.
3. A small number of subject industries account for the majority of high-amount filings, which would justify a typology-specific monitoring rule rather than blanket thresholds.

## Wireframe vs final dashboard

| Stage | File |
|-------|------|
| Low-fidelity wireframe | [`wireframes/low-fidelity/sar_dashboard_wireframe_v1.html`](wireframes/low-fidelity/sar_dashboard_wireframe_v1.html) |
| High-fidelity mock | [`wireframes/high-fidelity/sar_compliance_dashboard_v1.html`](wireframes/high-fidelity/sar_compliance_dashboard_v1.html) |
| Final Power BI report | [`reports/pbix/Regulatory_Suspicious_Activity_v1.pbix`](reports/pbix/Regulatory_Suspicious_Activity_v1.pbix) |

**What changed between wireframe and final.**

- The wireframe used a single "SAR list" page; the final report splits into Overview / Trends / Detail to give the daily-driver and the quarterly reviewer separate landing pages.
- A "narrative" page that tried to display SAR text was dropped — narratives are sensitive even synthetic ones, and the typology + amount + counts tell the operational story.
- An "agency × institution" matrix replaced two separate breakdowns; auditors specifically asked for that pivot.

## Open the report

- 📊 **Power BI file** — [`reports/pbix/Regulatory_Suspicious_Activity_v1.pbix`](reports/pbix/Regulatory_Suspicious_Activity_v1.pbix) *(Git LFS)*
- 📄 **PDF export** — `reports/exports/pdf/Regulatory_Suspicious_Activity_v1.pdf` *(add after first export)*

## Data &amp; privacy

All data in this project is **synthetic**. Subject identifiers are placeholders, narrative text is a stub string, and amounts are generated. No real SAR is referenced.

## Related

- 📁 Domain index: [`projects/banking-and-fintech/`](../)
- 📚 Repo standards: [naming](../../../docs/naming-conventions.md) · [Power BI](../../../docs/powerbi-best-practices.md) · [publishing checklist](../../../docs/publishing-checklist.md)
