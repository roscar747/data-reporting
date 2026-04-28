# Grid Load Forecasting &amp; DER Impact

<p>
  <img src="https://img.shields.io/badge/Domain-Energy%20%26%20Utilities-0F4C81?style=flat-square" alt="Domain"/>
  <img src="https://img.shields.io/badge/Status-v1-2E7D32?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/Data-Synthetic-F2C811?style=flat-square&labelColor=333" alt="Synthetic data"/>
  <img src="https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black" alt="Power BI"/>
</p>

> **One-line pitch.** Forecast net grid load in the presence of distributed energy resources (DER) and surface where solar, batteries and weather are most likely to push a feeder past its capacity.

<p align="center">
  <img src="reports/exports/screenshots/preview.png" alt="Grid Load Forecasting hero screenshot" width="100%"/>
</p>

---

## Business problem

Distribution network operators face a forecasting problem that has changed shape in the last five years: customers no longer just consume electricity, they generate it (rooftop solar) and store it (home batteries). The combined effect of weather, solar generation and battery dispatch on substation-level *net* load is harder to predict than gross load ever was, and the cost of getting it wrong is constraint events, voltage excursions and emergency curtailment.

This dashboard demonstrates how a Power BI report can sit on top of a forecasting pipeline (built upstream in Python or Spark, here represented by the synthetic dataset) to give planners a substation-by-substation view of where DER is most likely to bite next.

## Dataset description

| Table | Grain | Rows | File |
|-------|-------|------|------|
| `grid_load_der_impact_2023_2026` | One row per substation per timestamp | ~5,000 | [`data/raw/grid_load_der_impact_2023_2026.csv`](data/raw/grid_load_der_impact_2023_2026.csv) |

Schema: `timestamp`, `zip_code`, `substation_id`, `temp_celsius`, `cloud_cover_pct`, `humidity_pct`, `gross_grid_load_mw`, `der_solar_gen_mw`, `der_battery_discharge_mw`, `net_load_mw`, `voltage_sensor_pu`, `frequency_hz`.

**Source.** Synthetic. Substation IDs and zip codes are fabricated. Weather variables follow plausible UK climate ranges; DER penetration is modelled at varying levels per substation to expose contrasts.

**Time range covered.** 2023-01-01 through 2026-03-31.

**Refresh cadence.** Designed for 15-minute refresh against a SCADA / advanced-metering source; the published `.pbix` uses a static snapshot.

Full field definitions: [`docs/data-dictionary.md`](docs/data-dictionary.md).

## Key KPIs and insights

| KPI | What it answers | Target / threshold | Where it appears |
|-----|------------------|-------|------------------|
| Net load (MW) | Demand minus on-network generation | Within feeder capacity | Overview |
| Constraint risk | Probability that next-day forecast exceeds 90% of feeder capacity | Watch | Heatmap |
| DER offset | How much gross load DER is shaving | Higher = more grid stress relief | Trends |
| Voltage excursions | Count of intervals where `voltage_sensor_pu` is outside ±5% | 0 | Quality |
| Frequency events | Count of intervals where `frequency_hz` deviates >0.2 Hz | 0 | Quality |

Full DAX: [`docs/kpi-definitions.md`](docs/kpi-definitions.md).

**Headline insights.**

1. The constraint-risk heatmap concentrates on a specific ZIP-code cluster on summer afternoons — exactly the pattern that high-solar-penetration neighbourhoods produce when batteries are mostly discharged by 4pm.
2. Voltage excursions cluster around the same substations where DER offset is highest, suggesting DER coordination (not just additional capacity) would address the issue more cheaply than reinforcement.
3. Net load is becoming less correlated with temperature year-on-year — the dashboard makes that change visible, which has direct implications for how forecasting models should be retrained.

## Wireframe vs final dashboard

This project has three exploratory wireframes that show iteration on the heatmap visual — keep them all.

| Stage | File |
|-------|------|
| Low-fidelity wireframe (heatmap concept) | [`wireframes/low-fidelity/grid_constraint_capacity_heatmap_wireframe_v1.html`](wireframes/low-fidelity/grid_constraint_capacity_heatmap_wireframe_v1.html) |
| Low-fidelity wireframe (heatmap, v2) | [`wireframes/low-fidelity/grid_constraint_capacity_heatmap_dashboard_wireframe_v2.html`](wireframes/low-fidelity/grid_constraint_capacity_heatmap_dashboard_wireframe_v2.html) |
| Low-fidelity wireframe (forecast view) | [`wireframes/low-fidelity/grid_load_forecast_der_dashboard_wireframe_v1.html`](wireframes/low-fidelity/grid_load_forecast_der_dashboard_wireframe_v1.html) |
| Final Power BI report | [`reports/pbix/Grid_Load_Forecasting_and_DER_Impact_Analysis_v1.pbix`](reports/pbix/Grid_Load_Forecasting_and_DER_Impact_Analysis_v1.pbix) |

**What changed between wireframe and final.**

- The heatmap visual went through two passes (`_v1` and `_v2`) before settling on substation × hour-of-day rather than substation × calendar date — the seasonal pattern was clearer at hour-of-day granularity.
- Voltage and frequency quality KPIs were added late after a network-engineer review surfaced that load alone is necessary but not sufficient.
- A high-fidelity HTML mock was skipped because the final visuals map cleanly to native Power BI matrix and decomposition-tree visuals.

## Open the report

- 📊 **Power BI file** — [`reports/pbix/Grid_Load_Forecasting_and_DER_Impact_Analysis_v1.pbix`](reports/pbix/Grid_Load_Forecasting_and_DER_Impact_Analysis_v1.pbix) *(Git LFS)*
- 📄 **PDF export** — `reports/exports/pdf/Grid_Load_Forecasting_and_DER_Impact_Analysis_v1.pdf` *(add after first export)*

## Data &amp; privacy

All data in this project is **synthetic**. Substation identifiers, ZIP codes, and meter readings are fabricated.

## Related

- 📁 Domain index: [`projects/energy-and-utilities/`](../)
- 📚 Repo standards: [naming](../../../docs/naming-conventions.md) · [Power BI](../../../docs/powerbi-best-practices.md) · [publishing checklist](../../../docs/publishing-checklist.md)
