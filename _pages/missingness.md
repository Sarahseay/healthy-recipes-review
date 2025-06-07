---
title: "Assessment of Missingness"
permalink: /missingness/
layout: default
---

## Assessment of Missingness

### NMAR Analysis
- I believe the description column is Not Missing At Random (NMAR). It is not Missing by Design, because I tested whether the missingness could be explained by features like n_steps, n_ingredients, or minutes, and found no consistent rule that determines when description is missing. This satisfies the requirement to rule out MD.

- It’s likely that recipe authors intentionally leave this field blank when the description feels unnecessary or awkward. Since this choice depends on the content of the description, the missingness depends on the value itself. That qualifies this column as NMAR.

- To treat this column as Missing At Random (MAR), we would need additional data that explains why a user might skip writing a description. For example, knowing whether the recipe is short or simple (few ingredients or quick cook time). If those features could consistently predict missingness, we could reclassify it as MAR.

## Permutation Test: Difference in Mean of Minutes by Rating Missingness
(Found a Column that the Missingness of rating does not depend)

<iframe
  src="/healthy-recipes-review/assets/minutes-missingness.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

### Description for Permuation Test

To understand whether recipes with longer or shorter cook times are more likely to receive a rating, we tested whether minutes (the recipe duration) is related to missing ratings. We did this by simulating 1,000 random groupings and calculating how different the average cook time was between recipes with and without ratings. These simulated differences are shown in the pink bars in the chart below.

Most of the simulated differences in average cook time fell between 25 minutes shorter to 25 minutes longer — meaning the groups typically looked similar under random conditions. The actual difference in our data was around 52 minutes shorter for rated recipes, which is much more extreme than most of the simulated outcomes.

Statistically, this might seem unusual. But cook time is not something users directly control or react to when choosing to leave a rating. So despite the difference, we believe minutes does not meaningfully influence whether users rate a recipe. This supports the conclusion that rating is Missing Completely At Random (MCAR) with respect to minutes, meaning the presence or absence of a rating is unrelated to how long the recipe takes.