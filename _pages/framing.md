---
title: "Framing a Prediction Problem"
permalink: /framing/
layout: default
---

## Framing a Prediction Problem

- Framing a Prediction Problem We wanted to see if it’s possible to predict a recipe’s average user rating based on its nutrition and preparation details. Since the rating is a number (between 1.0 and 5.0), this is a regression problem so we are predicting a numeric outcome, not a category.

- Our response variable is avg_rating, which tells us how well a recipe was received by users. We chose it because it connects directly to our research question: Do healthier recipes get better reviews? In earlier analysis, we found a strong relationship between sugar and rating, showing that recipe features can influence how people rate them.

- To make fair predictions, we only used features that would be known before any users leave a review, things like prep time, number of steps, and nutritional facts (calories, sugar, fat, etc.).

- We’ll evaluate our model using Root Mean Squared Error (RMSE), which tells us how far off our predictions are. RMSE penalizes bigger mistakes more which is important since most ratings are close to 5.0 and even small differences matter.
