# JSC370 Final Project — Predicting Audience Movie Ratings

**Author:** Yujun Lu  •  **Course:** JSC370, Winter 2026

* **Website:** <https://alice2288.github.io/JSC370---2026Winter/>
* **Report (PDF):** [`report/report.pdf`](report/report.pdf) (also linked from the website)
* **Data source:** [The Movie Database (TMDb) REST API](https://developer.themoviedb.org/reference/intro/getting-started)

## What this is

The project asks whether we can predict how audiences rate a movie
just from its public TMDb metadata. Things like runtime, genre,
release year, language, vote count, and popularity. It builds on my
midterm exploratory analysis. The midterm found that no single
variable explains ratings well on its own. The final tries the
"throw everything in a tree-based model" answer to that question.

Two prediction tasks:

1. **Regression** — predict the continuous TMDb `vote_average` from
   movie metadata.
2. **Classification** — predict whether a film is *high-rated*
   (rating at least 7.0).

For each task I compare a linear baseline against Random Forest and
XGBoost.

## Repository layout

```
final-project/
├── analysis.qmd               # main analysis notebook (Quarto, Python)
├── collect_data.qmd           # one-time data collection from TMDb
├── data/
│   └── movies_raw.csv         # produced by collect_data.qmd (~1500 rows)
├── figures/                   # static PNGs the PDF report uses
│   ├── fig_rating_dist.png
│   ├── fig_corr.png
│   ├── fig_genre_box.png
│   ├── fig_pred_vs_actual.png
│   ├── fig_feature_importance.png
│   └── fig_roc.png
├── docs/                      # GitHub Pages website
│   ├── index.html
│   ├── viz.html
│   ├── viz_*.html             # standalone Plotly figures (8 of them)
│   └── report.pdf             # downloadable copy of the report
├── report/
│   ├── report.qmd             # Quarto source for the PDF
│   ├── report.pdf             # rendered PDF
│   └── tables/                # CSV metric tables, populated by analysis.qmd
├── requirements.txt
├── PRESENTATION_SCRIPT.md     # script for the 5-minute video
└── README.md
```

## How to run

From the `final-project/` directory:

```bash
# 1. install Python dependencies
pip install -r requirements.txt

# 2. collect movie metadata from TMDb (~5 minutes)
#    The API key from my midterm is hard-coded inside the qmd.
#    To use a different key, set TMDB_API_KEY in your shell, then
#    flip the YAML option `eval: false` to `eval: true` (or run the
#    cells interactively in RStudio / VS Code).
quarto render collect_data.qmd

# 3. run the full analysis end-to-end
#    Cleans data, engineers features, trains models, saves figures
#    to figures/, interactive plots to docs/, and metric CSVs to
#    report/tables/.
quarto render analysis.qmd

# 4. rebuild the PDF report
quarto render report/report.qmd
cp report/report.pdf docs/report.pdf

# 5. preview the website locally (optional)
python -m http.server 8000 -d docs
# → open http://localhost:8000/
```

If you don't have Quarto installed, get it from
<https://quarto.org/docs/get-started/>. It's the standard tool for
this course.

## Pushing to GitHub Pages

The project lives in this `final-project/` subfolder of the existing
`Alice2288/JSC370---2026Winter` repo. To get the website live:

1. Copy the contents of `final-project/docs/` into a top-level `/docs/`
   folder at the repo root (`cp -r final-project/docs/* ../docs/`).
2. On GitHub, open
   <https://github.com/Alice2288/JSC370---2026Winter/settings/pages>,
   set Source = `main` branch, folder = `/docs`, and save.
3. Wait about a minute. The page will be live at
   <https://alice2288.github.io/JSC370---2026Winter/>.

## Methods (one-paragraph summary)

I gathered metadata for about 1,500 unique films from the TMDb
`/discover/movie` endpoint using five sort strategies (recent
popular, classic popular, most-voted, top-rated, highest grossing)
and joined those with per-film details from `/movie/{id}`. Features
include log-transformed engagement (vote count, popularity) and
financial (budget, revenue) variables, runtime, release year and
month, language and country indicators, production-team size,
tagline and overview length, a `belongs_to_collection` flag, and 18
binary genre indicators. Models are trained with a 60 / 20 / 20
train / validation / test split (`random_state=370`) and evaluated
with R² / RMSE / MAE for regression and accuracy / ROC AUC for the
binary task.

## Deliverables checklist

| Item                                                  | Location                              |
|-------------------------------------------------------|---------------------------------------|
| Project description on website                        | `docs/index.html`                     |
| Eight interactive Plotly visualisations with captions | `docs/viz.html`                       |
| Downloadable PDF report                               | `docs/report.pdf`                     |
| Link to GitHub repo on the website                    | top of `index.html`                   |
| README at the top of the repository                   | this file                             |
| Data folder with the saved CSV                        | `data/`                               |
| 5-minute video walkthrough                            | linked from the website + Quercus     |

## Acknowledgements

* Course: JSC370 (Data Science II), University of Toronto, Winter 2026.
* Data: The Movie Database (TMDb). This product uses the TMDb API
  but is not endorsed or certified by TMDb.