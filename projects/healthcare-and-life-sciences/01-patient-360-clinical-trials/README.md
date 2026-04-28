# Patient 360 &amp; Clinical Trial Operational Funnel

<p>
  <img src="https://img.shields.io/badge/Domain-Healthcare%20%26%20Life%20Sciences-0F4C81?style=flat-square" alt="Domain"/>
  <img src="https://img.shields.io/badge/Status-v1-2E7D32?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/Data-Synthetic-F2C811?style=flat-square&labelColor=333" alt="Synthetic data"/>
  <img src="https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat-square&logo=powerbi&logoColor=black" alt="Power BI"/>
</p>

> **One-line pitch.** Combine a patient-level 360 view with a trial-site enrolment funnel and adverse-event safety velocity, so trial operations and medical-affairs can review the same source of truth.

<p align="center">
  <img src="reports/exports/screenshots/preview.png" alt="Patient 360 hero screenshot" width="100%"/>
</p>

---

## Business problem

Clinical trial operations (clin-ops) and medical affairs typically work with different reports — an enrolment funnel for clin-ops, a safety dashboard for medical. When the two are decoupled, a slow-enrolling site with a quietly rising adverse-event rate looks fine on either dashboard alone but bad on both together.

This project demonstrates a unified Patient 360 model that powers three views — enrolment funnel by site, adverse-event safety velocity by treatment arm, and a patient-level "pulse" view for medical monitors — sharing a single semantic model and one set of definitions.

## Dataset description

| Table | Grain | Rows | File |
|-------|-------|------|------|
| `clinical_trial_patient_360_dataset` | One row per patient per trial | ~5,000 | [`data/raw/clinical_trial_patient_360_dataset.csv`](data/raw/clinical_trial_patient_360_dataset.csv) |

Schema: `patient_uuid`, `birth_year`, `gender`, `trial_id`, `site_name`, `enrollment_date`, `last_visit_date`, `funnel_status`, `primary_diagnosis_code`, `is_adverse_event`, `treatment_arm`, `systolic_bp_avg`, `diastolic_bp_avg`, `lab_creatinine_mgdl`, `data_lineage_source`.

**Source.** Synthetic. Patient UUIDs are random, diagnosis codes follow public ICD-10 chapters (without representing any real patient), vitals follow clinically plausible ranges. The `data_lineage_source` column is included to demonstrate provenance tracking — a pattern useful for any regulated reporting.

**Time range covered.** Enrolment dates span 2023–2026.

Full field definitions: [`docs/data-dictionary.md`](docs/data-dictionary.md).

## Key KPIs and insights

| KPI | What it answers | Target / threshold | Where it appears |
|-----|------------------|-------|------------------|
| Enrolment rate (per site, per month) | How fast is each site recruiting? | Per-site target | Funnel |
| Funnel conversion (screening → enrolled) | What % of screened patients actually enrol? | ≥ 60% | Funnel |
| Adverse-event incidence | What % of patients have had a recorded AE? | Watch | Safety |
| Safety velocity (AE rate change MoM) | Is the AE rate accelerating? | Flat or down | Safety |
| Patient pulse (active vs disengaged) | Patients whose `last_visit_date` is overdue | Lower is better | Pulse |

Full DAX: [`docs/kpi-definitions.md`](docs/kpi-definitions.md).

**Headline insights.**

1. Two trial sites have above-average enrolment but below-average funnel conversion — a screening-criteria mismatch the dashboard exposes when site KPIs are viewed side by side.
2. AE safety velocity ticks up in one treatment arm two months before the absolute incidence number crosses any single-month threshold; the velocity measure surfaces the change earlier.
3. The patient-pulse view shows that disengaged patients cluster on Mondays — a scheduling artefact, not a clinical one, and easy to address operationally.

## Wireframe vs final dashboard

This project has four wireframes covering the three intended views; together they show how a Patient 360 model can be sliced for different audiences.

Each HTML wireframe has a **Live preview** link that renders the file directly in your browser via [htmlpreview.github.io](https://htmlpreview.github.io/) — no clone needed.

| Wireframe | Audience | File | Live preview |
|-----------|----------|------|--------------|
| Patient 360 (overall view) | Medical affairs | [`wireframes/low-fidelity/patient_360_dashboard_wireframe_v1.html`](wireframes/low-fidelity/patient_360_dashboard_wireframe_v1.html) | 🌐 [Open rendered wireframe](https://htmlpreview.github.io/?https://github.com/roscar747/data-reporting/blob/main/projects/healthcare-and-life-sciences/01-patient-360-clinical-trials/wireframes/low-fidelity/patient_360_dashboard_wireframe_v1.html) |
| Patient pulse (engagement) | Medical monitors | [`wireframes/low-fidelity/patient_pulse_dashboard_wireframe_v2.html`](wireframes/low-fidelity/patient_pulse_dashboard_wireframe_v2.html) | 🌐 [Open rendered wireframe](https://htmlpreview.github.io/?https://github.com/roscar747/data-reporting/blob/main/projects/healthcare-and-life-sciences/01-patient-360-clinical-trials/wireframes/low-fidelity/patient_pulse_dashboard_wireframe_v2.html) |
| Site enrolment funnel | Clin-ops | [`wireframes/low-fidelity/clinical_trial_site_enrollment_funnel_wireframe_v1.html`](wireframes/low-fidelity/clinical_trial_site_enrollment_funnel_wireframe_v1.html) | 🌐 [Open rendered wireframe](https://htmlpreview.github.io/?https://github.com/roscar747/data-reporting/blob/main/projects/healthcare-and-life-sciences/01-patient-360-clinical-trials/wireframes/low-fidelity/clinical_trial_site_enrollment_funnel_wireframe_v1.html) |
| AE safety velocity | Pharmacovigilance | [`wireframes/low-fidelity/ae_safety_velocity_dashboard_wireframe_v1.html`](wireframes/low-fidelity/ae_safety_velocity_dashboard_wireframe_v1.html) | 🌐 [Open rendered wireframe](https://htmlpreview.github.io/?https://github.com/roscar747/data-reporting/blob/main/projects/healthcare-and-life-sciences/01-patient-360-clinical-trials/wireframes/low-fidelity/ae_safety_velocity_dashboard_wireframe_v1.html) |
| Final Power BI report | All audiences | [`reports/pbix/Patient_360_and_Clinical_Trial_Operational_Funnel_v1.pbix`](reports/pbix/Patient_360_and_Clinical_Trial_Operational_Funnel_v1.pbix) | — |

**What changed between wireframe and final.**

- The `_v2` of the patient-pulse wireframe replaces a single big number with a small-multiples view by treatment arm — that's what made it onto the final page.
- AE safety velocity, originally a separate report in the wireframe set, became a page within the unified report once it was clear the same model could power it.
- A patient-level row-by-row table planned for the wireframe was demoted to a tooltip page in the final, since clinical reviewers said they would always start from a site or arm and drill in.

## Open the report

- 📊 **Power BI file** — [`reports/pbix/Patient_360_and_Clinical_Trial_Operational_Funnel_v1.pbix`](reports/pbix/Patient_360_and_Clinical_Trial_Operational_Funnel_v1.pbix) *(Git LFS)*
- 📄 **PDF export** — `reports/exports/pdf/Patient_360_and_Clinical_Trial_Operational_Funnel_v1.pdf` *(add after first export)*

## Data &amp; privacy

All data in this project is **synthetic**. Patient UUIDs, dates, vitals and diagnosis codes are fabricated. There is no PHI in this repository.

## Related

- 📁 Domain index: [`projects/healthcare-and-life-sciences/`](../)
- 📚 Repo standards: [naming](../../../docs/naming-conventions.md) · [Power BI](../../../docs/powerbi-best-practices.md) · [publishing checklist](../../../docs/publishing-checklist.md)
