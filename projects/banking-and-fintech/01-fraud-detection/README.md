# Real-Time Fraud Detection

<p>
  <img src="https://img.shields.io/badge/Domain-Banking%20%26%20Fintech-0F4C81?style=flat-square" alt="Domain"/>
  <img src="https://img.shields.io/badge/Status-v1-2E7D32?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/Data-Synthetic-F2C811?style=flat-square&labelColor=333" alt="Synthetic data"/>
  <img src="https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black" alt="Power BI"/>
</p>

> **One-line pitch.** Surface high-risk card transactions while they're still in flight, balanced against the cost of false positives that block legitimate customers.

<p align="center">
  <img src="reports/exports/screenshots/preview.png" alt="Real-Time Fraud Detection hero screenshot" width="100%"/>
</p>

---

## Business problem

Fraud teams at retail and challenger banks face a fundamental trade-off: every additional transaction blocked stops some fraudsters but also frustrates a measurable share of genuine customers. Decisions get made on rules tuned weeks ago, while patterns shift in days. Operational dashboards typically lag the threat by a full reporting cycle.

This project demonstrates the kind of dashboard that bridges the gap between data-science model output and the fraud operations floor. It scores transactions against a synthetic risk signal, exposes the rolling false-positive rate alongside the catch rate, and highlights the geographic and channel concentrations where rule-tuning would pay off most.

The audience is split between *operations* (who need the live signal queue) and *risk management* (who need confidence the model isn't drifting).

## Dataset description

| Table | Grain | Rows | File |
|-------|-------|------|------|
| `synthetic_fraud_detection_data_2023_2026` | One row per card transaction | ~5,000 | [`data/raw/synthetic_fraud_detection_data_2023_2026.csv`](data/raw/synthetic_fraud_detection_data_2023_2026.csv) |

Schema: `transaction_id`, `timestamp`, `customer_id`, `terminal_id`, `amount`, `transaction_type`, `card_type`, `entry_method`, `lat`, `lon`, `is_fraud`.

**Source.** Fully synthetic — generated to mimic the distribution of card-present and card-not-present transactions across a 2023–2026 window, with a fraud incidence rate calibrated to public benchmarks (single-digit basis points). Geo-coordinates are clustered around fictional metropolitan areas and do not correspond to any real terminals.

**Time range covered.** 2023-01-01 through 2026-03-31.

**Refresh cadence.** Modelled as a streaming source for the operations page; in this build the data is static, with a Power Query parameter that lets you toggle a "live mode" simulation.

Field-level documentation lives in [`docs/data-dictionary.md`](docs/data-dictionary.md).

## Key KPIs and insights

| KPI | What it answers | Target / threshold | Where it appears |
|-----|------------------|-------|------------------|
| Catch rate | What % of true fraud is flagged in flight? | ≥ 92% | Overview, Trends |
| False-positive rate | What % of legitimate transactions are wrongly flagged? | ≤ 0.5% | Overview, Trends |
| Loss avoided (£) | Currency value of fraud prevented this period | Higher is better | Overview |
| Decision latency (ms) | Time from transaction submit to decision return | ≤ 300 ms p95 | Operations |
| Top fraud terminals | Which terminals contribute most fraud volume? | n/a | Geography |

Full DAX definitions and business meaning: [`docs/kpi-definitions.md`](docs/kpi-definitions.md).

**Headline insights** (derived from the synthetic dataset).

1. Card-not-present transactions account for roughly two-thirds of the fraud volume despite a smaller share of total transactions — an obvious channel to harden first.
2. Fraud loss concentrates in three terminal clusters; rule-tuning by terminal-cluster rather than card-type produces a meaningfully higher catch rate at the same false-positive rate.
3. Decision latency remains under target across the window, so the operational bottleneck is not model serving — it is in the human review queue downstream.

## Wireframe vs final dashboard

| Stage | File |
|-------|------|
| Low-fidelity wireframe | [`wireframes/low-fidelity/fraud_detection_dashboard_wireframe_v1.html`](wireframes/low-fidelity/fraud_detection_dashboard_wireframe_v1.html) |
| High-fidelity mock | [`wireframes/high-fidelity/real_time_fraud_signals_dashboard_v1.html`](wireframes/high-fidelity/real_time_fraud_signals_dashboard_v1.html) |
| Final Power BI report | [`reports/pbix/Fraud_Detection_v1.pbix`](reports/pbix/Fraud_Detection_v1.pbix) |

**What changed between wireframe and final.**

- The low-fidelity wireframe placed the geographic terminal map as the hero visual; user feedback pulled the catch-rate / false-positive trend up to the top-left, since that's where operations leads look first.
- A separate "Decision latency" page was collapsed into a single card on Operations — the latency distribution didn't earn a full page on the synthetic data.
- Colour palette shifted from the wireframe's red/green binary to a sequential ramp; binary colour proved misleading when the model's confidence was anything other than extreme.

## Open the report

- 📊 **Power BI file** — [`reports/pbix/Fraud_Detection_v1.pbix`](reports/pbix/Fraud_Detection_v1.pbix) *(Git LFS, ~0.4 MB)*
- 📄 **PDF export** — `reports/exports/pdf/Fraud_Detection_v1.pdf` *(add after first export)*

## Data &amp; privacy

All data in this project is **synthetic**. There are no real card numbers, customer identifiers, terminals or geographic locations. The fraud labels are generated by a stochastic process that mirrors public-benchmark incidence rates.

## Tech notes

- **Semantic model.** Single fact (`fact_transaction`) with a generated `dim_date`, `dim_terminal`, `dim_card`, `dim_customer`. `is_fraud` lives on the fact for actuals; predicted fraud lives on a separate `fact_score` table to keep model output isolated from ground truth.
- **DAX patterns.** Time-intelligence via `SAMEPERIODLASTYEAR` for the catch-rate trend; rolling 7-day and 30-day windows via `CALCULATE` + `DATESINPERIOD`. Latency p95 calculated with `PERCENTILEX.INC`.
- **Power Query.** Source CSV typed at import; geo-coordinates split into a separate dimension for the map visual.

## Related

- 📁 Domain index: [`projects/banking-and-fintech/`](../)
- 📚 Repo standards: [naming](../../../docs/naming-conventions.md) · [Power BI](../../../docs/powerbi-best-practices.md) · [publishing checklist](../../../docs/publishing-checklist.md)
