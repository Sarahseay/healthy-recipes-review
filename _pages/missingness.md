---
title: "Assessment of Missingness"
permalink: /missingness/
---

## Assessment of Missingness

### NMAR Analysis
- I believe the description column is Not Missing At Random (NMAR). It is not Missing by Design, because I tested whether the missingness could be explained by features like n_steps, n_ingredients, or minutes, and found no consistent rule that determines when description is missing. This satisfies the requirement to rule out MD.

- It’s likely that recipe authors intentionally leave this field blank when the description feels unnecessary or awkward. Since this choice depends on the content of the description, the missingness depends on the value itself. That qualifies this column as NMAR.

- To treat this column as Missing At Random (MAR), we would need additional data that explains why a user might skip writing a description. For example, knowing whether the recipe is short or simple (few ingredients or quick cook time). If those features could consistently predict missingness, we could reclassify it as MAR.

## Permutation Test for rating based on the missingness of n_steps
(Found a Column that the Missingness of rating depends on)

<iframe
  src="/healthy-recipes-review/assets/nsteps-missingness.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

### Description for Permuation Test for Rating by Number of Steps:

To explore whether the missingness in the rating column depends on recipe characteristics in our dataset, we compared the distribution of number of steps (n_steps) between recipes with and without ratings. The overlaid histogram shows that recipes with ratings (rating_missing == False, color is blue) tend to have fewer steps on average and appear more frequently, while those with missing ratings (rating_missing == True, color is red) are less common and skew slightly toward higher step counts.

This difference in distributions suggests a potential dependency between n_steps and rating_missing. To evaluate this statistically, we ran a permutation test comparing the mean number of steps between the two groups. The result produced a p-value of 0.0, indicating that the observed difference is highly unlikely to occur by chance.

This analysis is similar to the approach used in Lecture 8, where a numerical variable’s distribution was compared by missing/not-missing groups using visuals and permutation testing. The plot and test provide strong evidence that the missingness of rating depends on n_steps. This supports the conclusion that rating is Missing At Random (MAR) with respect to recipe complexity, which directly informs our broader question of how recipe attributes relate to user engagement through ratings.