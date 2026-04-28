# Power BI in a portfolio repository

This guide documents how Power BI artifacts are managed in this repo: the file formats, what gets committed, what stays local, how to handle large reports, and how to expose reports to viewers who don't have Power BI Desktop.

## File formats and what to commit

| Format | What it is | Commit? | Notes |
|--------|-----------|---------|-------|
| `.pbix` | Packaged report (visuals + model + cached data) | Yes, via Git LFS | The "deliverable" form. Most reviewers will open this. |
| `.pbit` | Template (visuals + model, no data) | Yes, via Git LFS | Useful for sharing models without data. |
| `.pbip` | Power BI Project — folder of plain-text files (`.tmdl`, `.pbir`, JSON) | Yes, plain text | Diff-friendly. Reviewable in pull requests. |
| `.pbids` | Connection-only file with credentials hints | **No** | May leak server names. Listed in `.gitignore`. |
| `.pbi/localSettings.json`, `.pbi/cache.abf`, `.pbi/editorSettings.json` | Local Desktop state | **No** | Listed in `.gitignore`. |

The recommendation is: **publish both `.pbix` and `.pbip` when both are available.** `.pbix` opens in one click. `.pbip` makes the model and report readable without Power BI Desktop and gives meaningful Git diffs. The `BJF Group Revenue & Operations` project is a worked example of this dual-publishing pattern.

## Git LFS

Power BI files are binary and grow quickly once you embed visuals and cached data. The repo's `.gitattributes` tracks `*.pbix`, `*.pbit` and `*.parquet` through Git LFS so the main repo stays light.

Setup once per machine:

```bash
git lfs install
```

Cloning the repo:

```bash
git clone https://github.com/{{GITHUB_USER}}/data-reporting.git
cd data-reporting
git lfs pull          # materialize the .pbix files
```

Forgetting `git lfs install` before cloning leaves you with text-pointer files instead of real `.pbix` files. Power BI Desktop will refuse to open them with a confusing error — if that happens, run `git lfs install` then `git lfs pull`.

## Size limits

| Threshold | What to do |
|-----------|------------|
| `.pbix` < 25 MB | No special handling. LFS handles it. |
| `.pbix` 25–100 MB | Confirm cached data is necessary. Consider switching to a `.pbit` (template, no data) plus a separate dataset CSV. |
| `.pbix` > 100 MB | Don't commit. Host on OneDrive / SharePoint and link from the project README. The report has likely accumulated cached data that should be moved to a dataflow. |
| Single CSV > 50 MB | Don't commit raw. Sample down to <5 MB for the public repo, or LFS-track and link to the full dataset externally. |

GitHub's hard limit per file is 100 MB without LFS, 2 GB with LFS, and 5 GB per repository on the free tier.

## Versioning

Reports use a numeric major version in the filename (`Fraud_Detection_v1.pbix` → `Fraud_Detection_v2.pbix`). Within a major version, rely on Git history for incremental change tracking — don't rename to `_v1a`, `_v1_final`, `_v1_FINAL_FINAL`.

A new major version is warranted when:

- The semantic model changes shape (new tables, removed tables, new grain).
- A KPI's definition changes such that the same name now means something different.
- The visual design is rebuilt (not just tweaked).

When a new major version ships, keep the previous one in the same folder so reviewers can see the evolution. Update the project README to point to the latest as default and link the previous version below.

## Sharing publicly

Three options, in order of fidelity:

1. **Power BI service "Publish to web"** — produces an embeddable iframe. Anyone with the link can view, no login. *Only ever use this with synthetic data*. The link is in the project README under "Live report".
2. **PDF export** — `File → Export → Export to PDF` in Desktop. Produces one page per report page. Saved to `reports/exports/pdf/`. Good for offline reviewers and recruiters.
3. **PNG screenshots** — one per page, plus a hero `preview.png`. Saved to `reports/exports/screenshots/`. These are what render in the project README.

For the public sector / regulated-data audience, never use option 1 with anything other than fabricated data. Use options 2 and 3 plus the `.pbix` file gated behind a clone.

## Capturing screenshots that look professional

In Power BI Desktop:

1. Set the canvas size to *Custom: 1600 × 900* under *View → Page settings*. This gives you room for crisp PNG export and a 16:9 aspect ratio that fits GitHub's README width.
2. Hide the filter pane (*View → Filters off*) and the bookmark/selection panes before exporting.
3. Use *File → Export → Export to PDF* for a multi-page deliverable.
4. For per-page PNGs, use the system snipping tool against the canvas region rather than full-window screenshots — you'll get a clean rectangle without the Power BI chrome.
5. The hero image (`preview.png`) should be the most visually striking page of the report, cropped to 1600 × 900 or wider.

## Animated previews (GIFs)

A short GIF in the project README beats five static screenshots for catching attention. Keep them small.

| Tool | Notes |
|------|-------|
| ScreenToGif (Windows, free) | Best result-to-effort ratio. Record at 15 fps, trim to 10–15 s. |
| LICEcap (cross-platform, free) | Slightly older, but reliable. |
| Microsoft Clipchamp / OBS + ffmpeg | If you want video that GitHub will auto-play (use `.mp4` and link, since GIFs >5 MB stutter). |

Target <5 MB and <15 seconds. Place the GIF in `reports/exports/screenshots/walkthrough.gif` and link it from the README.

## Performance considerations

The `.pbix` files in this repo are intentionally small (<500 KB each) because the cached datasets are modest. If a report grows beyond a few MB without an obvious cause, run *View → Performance analyzer* in Desktop and look for:

- Visuals returning more rows than they need (cap with `TOPN` or a "Top 100" filter).
- DAX measures with `FILTER(ALL(...))` patterns that should be `CALCULATE(... , KEEPFILTERS(...))`.
- Bidirectional relationships you didn't intend.
- Imported tables that should be marked as *Date* tables to enable time-intelligence shortcuts.

The findings belong in the project's `docs/kpi-definitions.md` under a "Performance notes" section.

## Sanitizing before publishing

Before pushing a `.pbix` to a public repo, open it in Desktop and check:

- *Home → Transform data*: Power Query steps don't reference internal server names, file paths or credentials.
- *Modeling → Manage roles*: no internal email addresses or AD group names baked into RLS roles. Either remove the role or use synthetic placeholders.
- *Home → Refresh*: the report can render entirely from the synthetic CSVs in the repo. If it can't, you have a hidden dependency that won't reproduce for reviewers.
- *File → Options and settings → Data source settings*: clear cached credentials for any data source you don't want a reviewer to see referenced.

The [publishing checklist](publishing-checklist.md) wraps these into a single pre-flight gate.
