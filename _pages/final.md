---
title: "Final Model"
permalink: /final/
---

## Final Model

- To improve our model’s ability to predict recipe ratings, we engineered two new features grounded in earlier analysis.

- First, we created a binary feature called is_high_sugar, which flags whether a recipe’s sugar content is in the top 25% of the dataset ≥ 59g. This came from our Step 4 hypothesis test, where we found that perfect 5.0-rated recipes actually contained more sugar on average. By flagging high-sugar recipes, we allowed the model to more easily learn if high sugar ingredients were linked to better reviews.

- We also created a new categorical feature, cals_binned, by dividing the calorie column into four quantile-based groups: low, medium, high, and very high. A histogram in Step 2.2, that we tested and did not include in the write up, showed that calorie values were heavily right-skewed, so we binned the values to capture non-linear relationships more effectively. This strategy, supported by Lecture 14, helps models distinguish trends that wouldn’t be picked up by raw numerical values only.

- In addition, we included existing columns such as minutes, n_steps, and fat, which were included in step 6, as well as textual features like name, description, and ingredients. These text columns were encoded using TF-IDF, allowing our model to learn from recurring patterns in recipe names or ingredient lists that might signal higher ratings.

- For our final model, we chose Ridge Regression. Ridge is a regularized linear model that works especially well when dealing with many features, as it reduces overfitting by shrinking the impact of less important ones. This made it a good fit for our pipeline, which included both numeric and text features. We used GridSearchCV to tune the model’s alpha hyperparameter and found that alpha = 100 resulted in the best performance. All transformations like scaling, encoding, and vectorization, were implemented in a single pipeline.

- The final model achieved a root mean squared error (RMSE) of 0.6343, improving on both the baseline model from Step 6 (RMSE ≈ 0.6387) and the DummyRegressor (RMSE ≈ 0.6392). While the improvement was small, it demonstrated that thoughtful feature engineering, and not blindly applying transformations, to enhance predictive performance.

**Model A**

- Linear Regression
- Numeric features + is_high_sugar (binary)
- Text/categorical: name, tags, description, ingredients, cals_binned 
- Interpreation: Model A (is_high_sugar) 
- RMSE: 0.6343972321700903
- The model technically improved over the baseline, but the gain is very small.
- Either is_high_sugar and other features aren’t strongly predictive of ratings,
- Or the model (LinearRegression) might not be flexible enough to capture non-linear patterns.

*Model B*

- RandomForestRegressor cals_binned
- By binning calories into 4 quantile-based categories: “low”, “medium”, “high”, “very_high” answers: Is a low-calorie recipe rated higher than a very high-calorie one?
- Lecture 13–14: skewed numerical features should be binned or quantile-transformed

