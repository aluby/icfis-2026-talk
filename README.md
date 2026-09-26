# Remediable and Non-Remediable Flaws in Forensic Validation Studies

ICFIS 2026 talk slides and companion Shiny app — Amanda Luby and Maria Cuellar.
Responds to Chin et al. (2026), *Towards cumulative forensic science*.

- **Slides:** `index.qmd` → `docs/index.html` (self-contained revealjs; serve via GitHub Pages from `/docs`)
- **App:** `app.R` (Shiny; interactive version of the simulation)

## Layout

| Path | What it is |
|---|---|
| `index.qmd`, `icfis-saguaro.scss`, `references.bib` | deck source, theme, bibliography |
| `img/` | slide images; `_flowchart.qmd` is the inline-SVG flowchart included twice |
| `R/simulate_dgp.R`, `R/simulate_dgp_types.R` | the data-generating process used by the slides and the app |
| `data/*.csv` | pre-computed results the slides plot (see below) |
| `app.R` | Shiny app (sources `R/`) |

## Rebuilding

Packages: `tidyverse`, `here`, `NatParksPalettes`, `knitr`, `quarto` (slides); `shiny`, `dplyr`, `tidyr`, `purrr`, `ggplot2`, `DT` (app).

```bash
quarto render index.qmd   # writes docs/index.html
Rscript -e 'shiny::runApp("app.R")'
```

The slides do **not** run any long simulations except one small cached demo chunk; everything else reads `data/`.

## Provenance of `data/`

These files were generated in the research repo (`mariacuellar/validity-firearms`, folder `icfis/`), in this order:

1. `R/prepare_ames2_slide_data.R` (needs the raw Ames II spreadsheet, not included) → `ames2_firearm_counts.csv` (per-firearm response counts, Monson, Smith & Peters 2023)
2. `case_study_inconclusive.qmd` → `inconclusive_bounds.csv`
3. `sensitivity_nonrepresentative.qmd` → `sensitivity_nonrepresentative_grid.csv`

Numbers quoted in slide prose for the two case studies (e.g. FPR/FNR estimates) come from `case_study_inconclusive.qmd` and `case_study_nonrepresentative.qmd` in that repo; recheck them if either is re-run.

## Optional: static hosting of the app with shinylive

```r
# stage only what the app needs, then export
dir.create("/tmp/app/R", recursive = TRUE)
file.copy("app.R", "/tmp/app/"); file.copy(list.files("R", full.names = TRUE), "/tmp/app/R/")
shinylive::export("/tmp/app", "docs/app")   # then commit docs/app; Pages serves it at /app/
```

The first visit registers a service worker, so it needs one reload; the app then runs entirely in the browser.

## TODO before publishing

- The Shiny app needs a server host (shinyapps.io / Posit Connect Cloud)
