---
title: "Fairness Analysis"
permalink: /fairness/
layout: default
---

## Fairness Analysis

- We investigated whether our final model treats certain types of recipes unfairly by comparing its performance on two groups: high-sugar recipes and low-sugar recipes. We used an engineered feature from framing called `is_high_sugar`, which flags recipes in the top 25% of sugar content (≥ 59g). Group X consisted of high-sugar recipes, while Group Y included all others.

- To evaluate performance, we used our final Ridge regression model from framing without retraining or modification. This model was trained using both numeric and text-based recipe features — including sugar — and was evaluated using RMSE (root mean squared error), a standard metric for measuring prediction error in regression models.

- We calculated RMSE separately for each group. The model’s error for high-sugar recipes was 0.6522, while its error for low-sugar recipes was 0.6264. This resulted in an observed test statistic of 0.0258, indicating that the model makes noticeably larger prediction errors for indulgent, high-sugar recipes.

- To determine whether this difference was statistically significant, we defined the following hypotheses. The null hypothesis states that the model is fair: its RMSE is the same for both groups, and any difference is due to chance. The alternative hypothesis states that the model is unfair: it performs worse on high-sugar recipes, producing higher RMSE.

- We used the difference in RMSE (high-sugar minus low-sugar) as our test statistic and ran a permutation test by randomly shuffling the sugar group labels 1,000 times. For each shuffle, we recalculated the RMSE gap to simulate what would happen under the null hypothesis.

- Our observed RMSE difference of 0.0258 turned out to be unusually large. Only 0.2% of the permuted test statistics were greater than or equal to it, yielding a p-value of 0.002. This means it’s extremely unlikely that such a gap would occur by chance — strong evidence that the model’s performance is uneven.

- In conclusion, our model appears to be significantly less accurate when predicting ratings for high-sugar recipes, even though those recipes tend to be rated more highly by users (as shown in hypothesis). This finding reveals a fairness issue: the model may be biased against indulgent recipes, showing a disconnect between how users rate food and how well the model captures that pattern.

