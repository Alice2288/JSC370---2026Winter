# 5-Minute Presentation Walkthrough

Total runs about 4:50.
---

## Opening

Hi, I'm Yujun, and this is my JSC370 final project. The question
is whether audience movie ratings can be predicted from public
metadata alone. My midterm analysis on 148 TMDb films showed that
no single variable explains ratings well on its own. So for the
final, I scaled the dataset up to about fifteen hundred films and
framed it as two tasks: a regression to predict the continuous
rating, and a classification to predict whether a film is
high-rated, defined as rating seven or above.

---

## Data and methods

All data comes from the TMDb public API. I used the discover
endpoint with five sorting strategies, recent popular, classic
popular, most-voted, top-rated, and highest grossing, which gave
me about fifteen hundred films after deduplication. Features
include log vote count and log popularity, runtime, release year,
budget and revenue when reported, country and language flags, and
eighteen binary genre indicators. I fit three models: a linear or
logistic baseline, a Random Forest with 500 trees, and an XGBoost
with 600 boosting rounds, on a 60-20-20 train-validation-test
split with random state 370.

---

## Interactive figures (HW5)

### Figure 1. Rating vs vote count

**Mouse:** scroll to Figure 1, click the dropdown in the top-right
corner of the plot, pick Animation, then switch back to "All genres"

This figure plots vote average against the log of vote count, and
the dropdown lets me filter by genre. If I switch to Animation,
the points cluster toward the higher end of the rating scale. If
I switch back to all genres, the overall pattern is that more
votes by itself doesn't really push the rating up.

### Figure 2. Films by decade

This is the same data, but now over time, with each frame
showing one decade. When I hit play, the chart steps from the
seventies up to the twenty-twenties. Newer decades carry more
films and the bubbles spread to higher vote counts, but the
rating distribution itself stays pretty stable, mostly between
six and eight.

### Figure 3. Budget x Revenue x Rating in 3D

This last figure plots budget, revenue, and rating all in 3D.
When I drag to rotate the camera, it's clear that budget and
revenue track each other tightly, which is the diagonal sheet of
points. But rating doesn't lie along that diagonal. High-rated
and low-rated films can sit side by side at the same budget and
revenue level.

---

##  Modelling results

### Figure 7. Top features

Now for the modeling results. This figure shows which features
the regression model leans on, and the result surprised me. In
the midterm sample, log vote count looked dominant. But at
fifteen hundred films, the top of the ranking is taken over by
genre indicators like animation, drama, and music, plus the
English-language flag and release year. Log vote count drops to
the middle. So the model is picking up on systematic rating
differences across genres and languages, not on raw engagement.

### Figure 4. ROC curves

This is the ROC for the classification task, where I'm predicting
whether a film has rating at least seven. Random Forest and
XGBoost are basically tied at AUC around 0.87, while the logistic
baseline sits clearly below at 0.81. So there's a meaningful gain
from going non-linear, but switching from Random Forest to
XGBoost on top of that adds very little.

---

## Conclusions

I'm back on the homepage to wrap up the findings. There are two
main points. The first is that ratings are predictable from
public metadata to a meaningful degree. The tree models reach
R-squared around 0.55 on regression and AUC around 0.87 on
classification, while the linear baselines lag clearly behind, so
most of the signal sits in non-linear interactions.

The second is that scaling the dataset changed the story about
which features matter. At fifteen hundred films, the categorical
structure of the data, things like genre, language, and release
year, is what the model actually leans on, rather than the
engagement signals that looked dominant in the smaller midterm
sample.

There are a few caveats worth noting. Vote count is a post-release
quantity, so this isn't really a pre-release rating predictor. The
dataset skews English-language and US productions, which probably
inflates those flags. And TMDb raters are self-selected, not a
random sample of viewers.

---

## Wrap

The full PDF report is downloadable from the homepage, and all
the source is on the GitHub repo linked in the nav. The whole
project is built from three Quarto files and is fully
reproducible with random state 370. Thanks for watching.
