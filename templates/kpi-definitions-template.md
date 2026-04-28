<!--
  KPI DEFINITIONS TEMPLATE
  -------------------------
  One block per measure that surfaces in a visual. The goal is that a stakeholder
  can read the *Plain-English meaning* row and a peer engineer can read the *DAX*
  row and they both arrive at the same intent.
-->

# KPI &amp; measure definitions — {{Project Title}}

## Summary table

| KPI | Plain-English meaning | Unit | Target / threshold | Owner | Page |
|-----|------------------------|------|--------------------|-------|------|
| {{KPI 1}} | {{One short sentence}} | {{£ / count / % / hours}} | {{> 95%}} | {{Risk / Ops / Finance}} | {{Overview}} |
| {{KPI 2}} | {{…}} | {{…}} | {{…}} | {{…}} | {{…}} |

---

## {{KPI 1}}

**Plain-English meaning.** {{One paragraph aimed at a business reader.}}

**Why it matters.** {{What decision does this KPI feed? What happens if it moves the wrong way?}}

**Unit.** {{Currency / count / % / minutes}}

**Direction.** {{Higher is better / Lower is better / Stay within range}}

**Target / threshold.** {{e.g. ≥ 95% green, 90–95% amber, < 90% red. Source of the threshold if it's a regulatory or contractual one.}}

**Granularity.** {{Daily, monthly, by branch, by patient cohort, etc.}}

**Filter context expectations.** {{What the measure assumes about the filter context. E.g. "Always sliced by `dim_date[date]`; behaviour outside that context is undefined."}}

**DAX.**

```dax
{{KPI 1}} =
VAR _numerator = ...
VAR _denominator = ...
RETURN
    DIVIDE(_numerator, _denominator)
```

**Performance notes.** {{Anything that surfaced in Performance Analyzer — query time, storage engine vs. formula engine balance, optimization choices.}}

**Watch-outs.** {{Edge cases the visual hides — empty filter context, divide-by-zero, time-zone mis-handling, etc.}}

---

## {{KPI 2}}

(Repeat the block above for every KPI. Aim for completeness over brevity — this is the reference future-you will thank current-you for.)

---

## Calculation groups (if used)

| Group | Items | What it does |
|-------|-------|--------------|
| `Time Intelligence` | `Current`, `MTD`, `QTD`, `YTD`, `PY`, `YoY %` | Apply across any base measure to get a time-shifted variant. |

```dax
-- Item: YoY %
SELECTEDMEASURE() / CALCULATE(SELECTEDMEASURE(), SAMEPERIODLASTYEAR(dim_date[date])) - 1
```

## Implicit measures and base columns

List of any implicit measures (column aggregations) used in visuals so they don't get refactored away:

| Visual | Implicit measure | Recommendation |
|--------|------------------|----------------|
| {{Page / visual}} | `SUM(fact_x[amount])` | Promote to explicit measure `Total Amount`. |

## Change log

| Date | Change | Author |
|------|--------|--------|
| {{YYYY-MM-DD}} | Initial version. | {{Name}} |
