---
title: "Baseline Model"
permalink: /baseline/
layout: default
---

## Baseline Model

- We created a baseline for our predictions by training a simple model to estimate a recipe’s average user rating based only on its nutrition and preparation details: prep time (minutes), number of steps, and number of ingredients.

- We used 10 numerical features, including calories, sugar, fat, sodium, protein, prep time (minutes), number of steps, and number of ingredients. All of these features are quantitative, meaning they are measurable numeric values. We didn’t use any categorical features like description in this version, so there was no need for any encoding.

- To build the model, we used a process that first standardizes and normalizes each feature, then applies linear regression to predict a rating. We evaluated performance using k-fold cross-validation (k=5), which tests how well the model performs on unseen recipes. Our model achieved an average prediction error (RMSE) of 0.6387 stars.

- We also tested a dummy model that predicted the same average rating for every recipe, regardless of its data. The dummy model performed almost the same (RMSE = 0.6392), suggesting that the numeric features alone don’t explain much about how users rate recipes.

- This tells us that while nutrition data may hold some predictive value, it’s likely not enough on its own and the model may not be 'good' enough with these numerical features. Next, we will plan to add more meaningful features—like recipe tags or description in order to improve performance.
