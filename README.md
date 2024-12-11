# Cooking? I'm not good. 
#Hope it doesn't take Too long. 
#Better yet, Nutritious too.
- by Sandy Nguyen

---

## Overview

This data science project, foces on exploring the relationship of how long it takes to make a recipe and what rating would people give said recipe.
If duration has an effect on how many people are willing to try the recipe, will cooking duration influence the number of people that rate a recipe?

---

## Introduction

Food is vital to our survival, but let's make it enjoyable and satisfying to eat. If people have given their opinions already, then while it may not be the final guide, it is still a good reference. 
If duration does factor into a recipe's popularity or number of interactions, then it may affect how long people decide to make their recipes.


The first dataset, `recipe`, contains 83782 rows, indicating 83782 unique recipes, with 10 columns recording the following information:

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |

The second dataset, `interactions`, contains 731927 rows and each row contains a review from the user on a specific recipe. The columns it includes are:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |


## Data Cleaning and Exploratory Analysis

To make my analysis more convenient, I had to perform some DataFrame manipulations to clean the data.

To facilitate my investigation, 'nutrition' will be separated into different columns with values being floats. 

I performed a left merge between recipes and interactions in order to keep all recipes whether they were ever given a rating.

  - This step matches the unique recipe through their recipe id with the recipe's rating.

Any 0 values in rating were converted to null. I looked through the dataset and saw that even if they gave an amazing review or a good review with some caveats, the rating remained a 0.
  - Having these 0 that were not intentional of the reviewer could bias the results.

I added an average rating column, so that regardless of which row, we could see one individual person's rating alongside the overall receipe rating.

I also added an 'interactions' column which was the number of reviews that a recipe received, whether or not their was a rating.

Because the studied variable is 'minutes', I created a bool variable 'isduration30'. 
   - This is True when a recipe takes 30 minutes or order to make.
   - This is False when a recipe takes over 30 minutes to make.





