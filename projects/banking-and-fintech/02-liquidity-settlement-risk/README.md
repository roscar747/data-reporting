# Liquidity &amp; Settlement Risk

<p>
  <img src="https://img.shields.io/badge/Domain-Banking%20%26%20Fintech-0F4C81?style=flat-square" alt="Domain"/>
  <img src="https://img.shields.io/badge/Status-v1-2E7D32?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/Data-Synthetic-F2C811?style=flat-square&labelColor=333" alt="Synthetic data"/>
  <img src="https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black" alt="Power BI"/>
</p>

> **One-line pitch.** Track liquidity coverage, settlement-fail probability and counterparty risk against intraday tolerances, in one operational view.

<p align="center">
  <img src="reports/exports/screenshots/preview.png" alt="Liquidity & Settlement Risk hero screenshot" width="100%"/>
</p>

---

## Business problem

Treasury and operations teams in banks and central counterparties (CCPs) need a single pane of glass that joins three signals usually owned by different teams: intraday liquidity coverage, the probability that a trade will fail to settle on the value date, and the credit quality of the counterparties driving exposure. When those signals live in three different systems, surprises happen — typically late on a Friday before a long weekend.

This dashboard demonstrates how to unify the three signals into a model that supports same-day operational decisions (do I extend a credit line? do I net more aggressively?) and end-of-day regulatory comfort (am I within LCR tolerance?).

## Dataset description

| Table | Grain | Rows | File |
|-------|-------|------|------|
| `liquidity_settlement_risk_dataset_2023_2026` | One row per trade | ~5,000 | [`data/raw/liquidity_settlement_risk_dataset_2023_2026.csv`](data/raw/liquidity_settlement_risk_dataset_2023_2026.csv) |

Schema: `trade_id`, `execution_timestamp`, `settlement_date`, `counterparty_id`, `counterparty_risk_rating`, `asset_class`, `currency`, `notional_amount`, `liquidity_haircut`, `lcr_impact_value`, `settlement_status`, `netting_eligible`, `settlement_lag_days`, `fail_probability`.

**Source.** Synthetic. Trade volumes and asset-class distributions are calibrated to a mid-sized custodian's typical weekly profile. Counterparty ratings follow a long-tail distribution roughly aligned with rating-agency public buckets.

**Time range covered.** 2023-01-01 through 2026-03-31.

**Refresh cadence.** Designed for hourly refresh against an upstream warehouse; the published `.pbix` uses a static snapshot.

Full field definitions: [`docs/data-dictionary.md`](docs/data-dictionary.md).

## Key KPIs and insights

| KPI | What it answers | Target / threshold | Where it appears |
|-----|------------------|-------|------------------|
| Liquidity Coverage Ratio (LCR) | Is high-quality liquid asset coverage above the regulatory floor? | ≥ 100% | Overview |
| Net liquidity outflow | Expected outflow over a 30-day stress horizon | Watch | Trends |
| Settlement-fail rate | % of trades failing to settle on value date | ≤ 1.0% | Overview |
| Expected fail loss | Notional × fail probability, summed | Lower is better | Detail |
| Counterparty concentration | Notional held against the top 10 counterparties | ≤ 25% | Counterparty |

Full DAX: [`docs/kpi-definitions.md`](docs/kpi-definitions.md).

**Headline insights.**

1. Settlement-fail probability spikes around quarter-ends — a signal that the netting engine should ramp aggression in the last three business days of the quarter.
2. Counterparty concentration in the lowest two ratings buckets accounts for a disproportionate share of expected fail loss; a simple haircut adjustment cuts EFL meaningfully without changing exposure mix.
3. LCR tracks comfortably above the 100% floor but with seasonality — the dashboard exposes the lows that monthly reporting hides.

## Wireframe vs final dashboard

The HTML wireframes have a **Live preview** link that renders the file directly in your browser via [htmlpreview.github.io](https://htmlpreview.github.io/) — no clone needed.

| Stage | File | Live preview |
|-------|------|--------------|
| Low-fidelity wireframe | [`wireframes/low-fidelity/liquidity_settlement_risk_dashboard_wireframe_v1.html`](wireframes/low-fidelity/liquidity_settlement_risk_dashboard_wireframe_v1.html) | 🌐 [Open rendered wireframe](https://htmlpreview.github.io/?https://github.com/roscar747/data-reporting/blob/main/projects/banking-and-fintech/02-liquidity-settlement-risk/wireframes/low-fidelity/liquidity_settlement_risk_dashboard_wireframe_v1.html) |
| High-fidelity mock | [`wireframes/high-fidelity/liquidity_settlement_risk_dashboard_v1.html`](wireframes/high-fidelity/liquidity_settlement_risk_dashboard_v1.html) | 🌐 [Open rendered wireframe](https://htmlpreview.github.io/?https://github.com/roscar747/data-reporting/blob/main/projects/banking-and-fintech/02-liquidity-settlement-risk/wireframes/high-fidelity/liquidity_settlement_risk_dashboard_v1.html) |
| Final Power BI report | [`reports/pbix/Liquidity_and_Settlement_Risk_v1.pbix`](reports/pbix/Liquidity_and_Settlement_Risk_v1.pbix) | — |

**What changed between wireframe and final.**

- LCR was a single big number in the wireframe; it became a gauge with a 30-day trend sparkline once stakeholders said "I want to know it's been ok all month, not just right now."
- Counterparty concentration moved from a treemap to a stacked bar — easier to compare ratings buckets side by side.
- Asset class drill-through was removed from the front page and pushed into a tooltip page; it cluttered the headline view.

## Open the report

- 📊 **Power BI file** — [`reports/pbix/Liquidity_and_Settlement_Risk_v1.pbix`](reports/pbix/Liquidity_and_Settlement_Risk_v1.pbix) *(Git LFS)*
- 📄 **PDF export** — `reports/exports/pdf/Liquidity_and_Settlement_Risk_v1.pdf` *(add after first export)*

## Data &amp; privacy

All data in this project is **synthetic**. Trades, counterparty IDs, ratings and notionals are fabricated.

## Related

- 📁 Domain index: [`projects/banking-and-fintech/`](../)
- 📚 Repo standards: [naming](../../../docs/naming-conventions.md) · [Power BI](../../../docs/powerbi-best-practices.md) · [publishing checklist](../../../docs/publishing-checklist.md)
