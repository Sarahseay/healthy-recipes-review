---
title: "Healthy Recipes, Highly Rated?"
layout: default
---

## Introduction

This project explores the Recipes and Ratings dataset from Food.com, which contains tens of thousands of user-submitted recipes along with nutrition facts and review scores.

Our main question:

**Do healthier recipes tend to receive higher ratings?**

- To explore this, we analyzed the relationship between user ratings and recipe nutrition, including sugar, fat, and calories. This site walks through our entire process, including cleaning, hypothesis testing, and prediction modeling.
- This project explores the Recipes and Ratings dataset from Food.com, which contains tens of thousands of user-submitted recipes along with nutrition facts and review scores.
- This question is important because it explores if user preferences align with nutrition. If healthier recipes tend to receive higher ratings, it could reflect a growing interest in wellness and clean eating. If not, it may suggest that taste, indulgence, or convenience outweigh nutrition based on users evaluation of recipes. These insights could be valuable for food platforms, recipe developers, and health-focused recommendation systems.
- 234429 Rows.
- These columns are relevant to our question: id: The recipe idenitfier name: Name of the recipe minutes: Prep Time in Minutes tags: Tags for the recipe n_steps: Number of steps in the recipe steps: The recipes steps in order description: User provided description of the recipe nutrition: Nutrition information in the form :calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV). PDV means the percentage of daily value. ingredients: The ingredients used in the recipe n_ingredients: The number of indredients needed for the recipe
- The other columns will be cleaned in the Data Cleaning Process

----

## Data Cleaning and Exploratory Data Analysis

### Preview of DataFrame

| name                                 | avg_rating | calories | fat | sugar | protein | n_ingredients |
|:------------------------------------|-----------:|---------:|----:|------:|--------:|--------------:|
| 1 brownies in the world best ever   |          4 |    138.4 |  10 |    50 |       3 |             9 |
| 1 in canada chocolate chip cookies  |          5 |    595.1 |  46 |   211 |      13 |            11 |
| 412 broccoli casserole              |          5 |    194.8 |  20 |     6 |      22 |             9 |

### How We Cleaned the Data

- Removed rows with unrealistic values like 0-minute prep times or over 1-year durations.
- Dropped irrelevant columns such as contributor IDs, submission dates, and redundant time/user info.
- Filled missing descriptions and standardized formatting in text columns.
- Converted ingredient strings to Python lists for analysis and verified that ingredient counts matched.
- Removed ~2,600 recipes with no ratings, as they didn’t help answer our research question.
- Split the `nutrition` column into calories, fat, sugar, sodium, protein, saturated fat, and carbs.
- Trimmed the top 0.3% outliers in each nutrition column to remove extreme values while keeping healthy options like sugar-free drinks or 0-fat fruit.

---

### Univariate Analysis

#### Bar Chart of Recipe Rating Groups

<iframe src="/healthy-recipes-review/assets/rating-group-bar.html" width="800" height="500" frameborder="0"></iframe>

Most recipes are rated “High” or “Max,” showing that users tend to leave favorable feedback.

#### Histogram of Sugar

<iframe src="/healthy-recipes-review/assets/sugar-hist.html" width="800" height="500" frameborder="0"></iframe>

Sugar values are heavily right-skewed. Most recipes are under 60g of sugar, but a few outliers go beyond 500g.

---

### Bivariate Analysis

#### Box Plot: Calories by Rating Group

<iframe src="/healthy-recipes-review/assets/calories-box.html" width="800" height="500" frameborder="0"></iframe>

Each rating group shows a similar calorie range and median. There’s no clear link between calories and how users rate recipes.

---

### Interesting Aggregates

#### Table: Average Calories and Sugar by Rating Group

| rating_group | calories (mean) | calories (median) | sugar (mean) | sugar (median) |
|:-------------|----------------:|------------------:|-------------:|---------------:|
| High         |         382.48  |            296.9  |       49.91  |             19 |
| Low          |         401.05  |            306.0  |       65.68  |             26 |
| Max          |         381.51  |            293.8  |       57.02  |             23 |
| Medium       |         398.82  |            313.6  |       53.71  |             22 |

Higher-rated recipes tend to have lower average sugar and calorie values, while lower-rated recipes are more indulgent on average.

-----
## Assessment of Missingness

### NMAR Analysis
- The `description` column appears Not Missing At Random (NMAR). No clear rule (e.g., based on `n_steps`, `minutes`, or `n_ingredients`) explains when it's missing, ruling out Missing by Design. It’s likely that users omit it when they feel it’s unnecessary, meaning the missingness depends on the value itself.

- To classify it as MAR, we'd need more data showing that simple recipes or short durations predict missingness — but we don’t currently see strong evidence for that.

---

### Permutation Test for `rating` Missingness by `n_steps`

<iframe
  src="/healthy-recipes-review/assets/nsteps-missingness.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

We compared `n_steps` between recipes with and without ratings using an overlaid histogram. Recipes with ratings are more common and cluster at lower step counts, while those without ratings show a slight shift toward longer recipes.

A permutation test confirmed this visual trend, producing a p-value of 0.0 — meaning the difference is unlikely due to chance. This suggests that the missingness of `rating` depends on `n_steps`, so we classify it as Missing At Random (MAR).

----

## Hypothesis Testing

We wanted to explore whether healthier recipes receive higher ratings. Specifically, we tested whether recipes with perfect 5.0 ratings contain less sugar than recipes with lower ratings.

### Question
**Do recipes with perfect 5.0 ratings contain less sugar than recipes with lower ratings?**

We grouped recipes into:
- **Perfect5.0**: avg rating = 5.0  
- **LessThan5.0**: avg rating < 5.0

### Hypotheses
- **Null Hypothesis**: There is no relationship between rating and sugar content.
- **Alternative Hypothesis**: Perfect-rated recipes contain less sugar on average.

### Permutation Test

We used the difference in group means as the test statistic:  
**mean_sugar_LessThan5.0 – mean_sugar_Perfect5.0**
- **Observed difference**: –5.88 grams  
- **α = 0.05**

We ran a permutation test with 1,000 simulations by shuffling the rating labels to generate a null distribution of differences.

### Interpretation of the Null Plot

The null distribution centers near 0, while our observed difference of –5.88 (red line) lies far in the left tail. The **p-value = 0.0000**, indicating the observed result is highly unlikely under the null.

### Conclusion

We reject the null hypothesis. Rather than having less sugar, recipes with perfect ratings actually have **more** sugar on average. This strong association suggests that sweeter recipes may receive better reviews, though this does not imply causality.

----

### Framing a Prediction Problem

We framed our task as a regression problem, since we are predicting a numeric outcome — a recipe’s average user rating (avg_rating), which ranges between 1.0 and 5.0.

Our response variable is avg_rating, which directly answers our research question: Do healthier recipes get better reviews? Earlier analysis showed a link between sugar and rating, suggesting that recipe features do influence user reviews.

To avoid data leakage, we only used features available before a recipe receives any reviews — such as prep time, number of steps, and nutritional information (calories, sugar, fat, etc.).

To evaluate our models, we used Root Mean Squared Error (RMSE), which penalizes larger mistakes and is sensitive to the fact that most ratings cluster near 5.0, so small prediction errors are important.

---

### Baseline Model

We created a baseline model to predict a recipe’s average rating using only numeric features like calories, sugar, fat, sodium, protein, prep time, number of steps, and number of ingredients — all of which are quantitative and didn’t require encoding.

We used a linear regression pipeline with StandardScaler and evaluated performance using 5-fold cross-validation. The model’s average RMSE was 0.6387, showing how far off the predictions were on average.

To compare, we used a dummy model that predicted the same rating for every recipe. Its RMSE was 0.6392, nearly identical to our baseline model, which suggests that these numeric features alone don’t provide strong predictive power.

This result indicates that while nutrition and prep details are helpful, they’re not enough. We’ll need to engineer better features (like tags or descriptions) to improve future model performance.

----

### Final Model

To improve our predictions, we engineered two new features based on earlier findings. First, we created is_high_sugar, which flags recipes in the top 25% of sugar (≥ 59g). This came from our hypothesis test in Step 4, where we found that higher sugar content was linked to perfect 5.0 ratings.

We also created cals_binned, a categorical version of the calorie column, dividing values into four quantile-based bins (low, medium, high, very high). This helped the model capture non-linear relationships more effectively than raw calorie values.

Our final model used Ridge Regression with both numeric and text features. We included prep time, steps, fat, is_high_sugar, and TF-IDF encodings of name, description, and ingredients. We used a pipeline to combine all preprocessing and tuned the regularization strength (alpha) with GridSearchCV, selecting alpha=100.

The Ridge model achieved RMSE = 0.6343, improving on both the baseline model (0.6387) and DummyRegressor (0.6392). While the improvement was modest, it shows the impact of thoughtful feature engineering.

----

### Fairness Analysis

We evaluated whether our final model was fair by comparing its performance on two groups: high-sugar recipes (is_high_sugar = 1) and low-sugar ones (is_high_sugar = 0). This feature flagged recipes in the top 25% of sugar content (≥ 59g), based on findings from our earlier analysis.

We used the final Ridge regression model without retraining, and calculated RMSE separately for both groups. The model's error was 0.6522 for high-sugar recipes and 0.6264 for low-sugar recipes, leading to an observed RMSE gap of 0.0258.

To test if this difference was significant, we ran a permutation test: we shuffled sugar group labels 1,000 times and recalculated the RMSE difference for each. Our null hypothesis assumed the model is fair (no true difference in errors), and the alternative stated the model is unfair (worse on high-sugar recipes).

Interpretation of Fairness Test Plot
The red dashed line shows our observed RMSE gap. Only 0.2% of permuted test statistics were more extreme, giving a p-value of 0.002. This result strongly suggests that the model performs worse on high-sugar recipes — even though those often receive perfect user ratings.

We conclude that our model exhibits unfair behavior, systematically underperforming on indulgent recipes. This reveals a disconnect between user preferences and model predictions, pointing to a fairness concern in recipe rating predictions.