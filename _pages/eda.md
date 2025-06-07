---
title: "Data Cleaning and Exploratory Data Analysis"
permalink: /eda/
layout: default
---

## Data Cleaning and Exploratory Data Analysis


## Preview of DataFrame

| name                                 |   avg_rating |   calories |   fat |   sugar |   protein |   n_ingredients |
|:-------------------------------------|-------------:|-----------:|------:|--------:|----------:|----------------:|
| 1 brownies in the world    best ever |            4 |      138.4 |    10 |      50 |         3 |               9 |
| 1 in canada chocolate chip cookies   |            5 |      595.1 |    46 |     211 |        13 |              11 |
| 412 broccoli casserole               |            5 |      194.8 |    20 |       6 |        22 |               9 |
| 412 broccoli casserole               |            5 |      194.8 |    20 |       6 |        22 |               9 |
| 412 broccoli casserole               |            5 |      194.8 |    20 |       6 |        22 |               9 |

## Data Cleaning Explaination:
* We found one recipe that listed 0 minutes, which isn’t realistic, so we removed it.
* Dropped irrelevant columns like contributor_id (who submitted the recipe) and submitted (timestamp), time, user_id, recipe_id, since they don’t affect healthiness or ratings.
* We also found a small number of extreme outliers, some recipes claimed to take over a year to prepare, likely due to errors. So to keep the data reasonable, we kept only recipes that take between 1 minute and 36 hours to make.
* The contributor_id column identifies who submitted each recipe, we’re not studying user behavior or who made the recipe, so we can remove this column.
* he submitted column shows when each recipe was added to the website,we’re not analyzing trends over time, this column wasn’t needed so we can remove it.
* changing the data type for nutrition so we can better anaylsis it
* Cleaning the description column by filling in missing values and replacing meaningless entries like '.' with a standard placeholder: 'No description provided'. This ensures all recipes have readable, usable descriptions.
* Cleaned the ingredients column by converting it from a string that looked like a list into an actual Python list, so we could count and analyze ingredients more easily.
* We confirmed that the n_ingredients column accurately reflects the number of ingredients in each recipe. It’s technically redundant but useful, so we kept it. 
* Some recipes had no ratings (about 2,600 rows), meaning no users had reviewed them. Since we’re analyzing what makes recipes highly rated, we removed these.
* We kept only the columns that were directly useful for analyzing recipe healthiness and ratings. These include the recipe name, prep time, ingredients, number of ingredients and steps, description, nutrition details (split into calories, fat, sugar, etc. in a new data frame), tags, and the average rating. Everything else, like who submitted the recipe or when, wasn’t relevant to our question, so we removed it to keep the data focused and clean.
* We created a new dataset, nutri_df, to focus on nutritional analysis. We split the original nutrition column into 7 separate columns: calories, fat, sugar, sodium, protein, saturated fat, and carbohydrates to make each value easier to study.
* We found most recipes had under 3,638 calories. To keep things realistic, we removed a few extreme cases and recipes with 0 calories, which were likely errors.
* We removed the top 0.3% of recipes with extremely high fat values, to focus on realistic everyday meals. While we filtered out those rare high-fat dishes, we kept 0-fat recipes like tea and fruit since they’re valid and reflect healthier options.
* Some recipes had high sugar, so we trimmed the top 0.3% while keeping sugar-free options like sauces and alcohol.
* We removed only the most extreme sodium values, so we trimmed the top 0.3% while keeping healthy, no-salt meals.
* We filtered out a few outliers so we trimmed the top 0.3%, keeping valid low-protein recipes like drinks and desserts.
* We excluded a few rare cases with very high saturated fat, so we trimmed the top 0.3% to focus on more balanced meals.
* Carbs above 201g were rare and unrealistic so so we trimmed the top 0.3%, but we kept low-carb dishes like meat and broth.

---
### Univariate Analysis

## Bar Chart of Recipe Rating Groups

<iframe
  src="/healthy-recipes-review/assets/rating-group-bar.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

### Interpretation of Bar Chart

This bar chart shows how recipes are distributed across rating categories. Most recipes are rated highly, with the majority falling into the “Max” and “High” groups, suggesting a trend toward positive user feedback.

## Histogram of Sugar

<iframe
  src="/healthy-recipes-review/assets/sugar-hist.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

### Interpretation of Histogram of Sugar

Most recipes fall under 60g of sugar, but a few have extremely high values, forming a right-skewed distribution. This helps us spot outliers and understand the variation in recipe healthiness.

---

## Bivariate Analysis

## Box Plot (Calories vs. Rating Group)

<iframe
  src="/healthy-recipes-review/assets/calories-box.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

### Interpretation of Box Plot

 We compared calorie counts across different rating groups to explore whether healthier (lower-calorie) recipes received higher ratings. The boxplot shows that all groups have similar calorie medians and wide spreads, with many outliers above 1000 calories. This suggests there is no strong relationship between calorie content and recipe ratings — users appear to rate high- and low-calorie recipes similarly.

---

## Interesting Aggregates

## Table: Average Calories and Sugar by Rating Group

| rating_group   |   ('calories', 'mean') |   ('calories', 'median') |   ('calories', 'count') |   ('sugar', 'mean') |   ('sugar', 'median') |   ('sugar', 'count') |
|:---------------|-----------------------:|-------------------------:|------------------------:|--------------------:|----------------------:|---------------------:|
| High           |                382.484 |                    296.9 |                   83989 |             49.9095 |                    19 |                83989 |
| Low            |                401.046 |                    306   |                   11299 |             65.6844 |                    26 |                11299 |
| Max            |                381.513 |                    293.8 |                   98236 |             57.0196 |                    23 |                98236 |
| Medium         |                398.821 |                    313.6 |                   34822 |             53.7125 |                    22 |                34822 |


### Interpretation of Table

This table shoes that recipes with the highest user ratings (“High”) had the lowest average calories and sugar, while lower-rated recipes showed the opposite trend. This table provides concrete values that highlight how calorie and sugar content vary across rating levels, helping us understand patterns in recipe nutrition.

