---
title: "Hypothesis Testing"
permalink: /hypothesis/
layout: default
---

## Hypothesis Testing

- We wanted to explore whether healthier recipes receive higher ratings. Specifically, we tested whether recipes with perfect 5.0 ratings contain less sugar than recipes with lower ratings.
We ask the Question:
Do recipes with perfect 5.0 ratings contain less sugar than recipes with lower ratings?

- To do this we grouped recipes into:
Perfect5.0: recipes with an average rating of 5.0
LessThan5.0: recipes with an average rating below 5.0

- Hypotheses:

- Null Hypothesis: There is no relationship between a recipe’s rating and its sugar content. Any observed difference in average sugar between perfect-rated and lower-rated recipes is due to random chance.

- Alternative Hypothesis: Recipes with perfect 5.0 ratings contain less sugar on average than recipes with lower ratings.

- Permutation Test:
We used a difference in average sugar to compare the two groups as our test statistic:
mean_sugar_LessThan5.0 – mean_sugar_Perfect5.0
This means we compared average sugar content between the two groups.

- This setup allows us to test whether perfect-rated recipes tend to be lower in sugar.
Observed difference: –5.88 grams.
This means recipes with a perfect 5.0 rating had 5.88 grams more sugar than recipes with lower ratings.
Significance level (α): 0.05

- We conducted a permutation test with 1,000 repetitions, randomly shuffling the rating labels each time to simulate what kinds of sugar differences we'd expect if ratings and sugar were unrelated.

- Null distribution visualization:
The plot shows the simulated distribution of differences in mean sugar we would expect by chance. The simulated null distribution was centered around 0, and most values fell between –2 and +2 grams.
Our observed value of –5.88 was far in the red dashed line, it falls far in the left tail, outside the range of normal variation under the null.
And our p-value = 0.0000. 
(insert plot)

- Conclusion:
We found strong evidence to reject the null hypothesis.  A difference as extreme as -5.88 grams is very unlikely to occur by chance. This means rather than being lower in sugar, recipes with perfect ratings actually had more sugar on average than those with lower ratings. A difference as extreme as –5.88 grams is very unlikely to occur by chance.
We cannot conclude why this pattern exists as other factors may contribute, and statistical significance does not imply causality. We can say that there is a strong association between sugar content and average rating in this dataset.
