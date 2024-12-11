# Investigation on the Relationship between Recipe Cooking Duration and Its Rating

## Cooking? I'm not good. 
## Hope It Doesn't Take Too Long. 
## Better Yet, Nutritious Too.
- by Sandy Nguyen

---

## Overview

This data science project, foces on exploring the relationship of how long it takes to make a recipe and what rating would people give said recipe.
If duration has an effect on how many people are willing to try the recipe, will cooking duration influence the number of people that rate a recipe?

Okay, so the exposure of the recipe is increased, but how's the rating doing?

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
   - It is somewhat an arbritrary number but one that could be argued to be a moderately quick recipe to make.

For later activities, I also created a 'prop_interact' 
  - which is the number of interactions a recipe received over the number of total recipe interactions
and 'is_rating_missing'
  - to study the samples with or without a rating for the missingness mechanic.


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>id</th>
      <th>minutes</th>
      <th>contributor_id</th>
      <th>submitted</th>
      <th>tags</th>
      <th>n_steps</th>
      <th>steps</th>
      <th>description</th>
      <th>ingredients</th>
      <th>n_ingredients</th>
      <th>calories</th>
      <th>total_fat_(PDV)</th>
      <th>sugar_(PDV)</th>
      <th>sodium_(PDV)</th>
      <th>protein_(PDV)</th>
      <th>saturated_fat_(PDV)</th>
      <th>carbohydrates_(PDV)</th>
      <th>user_id</th>
      <th>recipe_id</th>
      <th>date</th>
      <th>rating</th>
      <th>review</th>
      <th>ave_rating</th>
      <th>interactions</th>
      <th>isduration30</th>
      <th>prop_interact</th>
      <th>is_rating_missing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1 brownies in the world    best ever</td>
      <td>333281</td>
      <td>40</td>
      <td>985201</td>
      <td>2008-10-27</td>
      <td>['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings']</td>
      <td>10</td>
      <td>['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']</td>
      <td>these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!</td>
      <td>['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour']</td>
      <td>9</td>
      <td>138.4</td>
      <td>10.0</td>
      <td>50.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>19.0</td>
      <td>6.0</td>
      <td>386585.0</td>
      <td>333281.0</td>
      <td>2008-11-19</td>
      <td>4.0</td>
      <td>These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.</td>
      <td>4.0</td>
      <td>1</td>
      <td>False</td>
      <td>0.000004</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1 in canada chocolate chip cookies</td>
      <td>453467</td>
      <td>45</td>
      <td>1848091</td>
      <td>2011-04-11</td>
      <td>['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']</td>
      <td>12</td>
      <td>['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft &amp; chewy in the center', 'serve hot and enjoy !']</td>
      <td>this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.</td>
      <td>['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']</td>
      <td>11</td>
      <td>595.1</td>
      <td>46.0</td>
      <td>211.0</td>
      <td>22.0</td>
      <td>13.0</td>
      <td>51.0</td>
      <td>26.0</td>
      <td>424680.0</td>
      <td>453467.0</td>
      <td>2012-01-26</td>
      <td>5.0</td>
      <td>Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, &amp; I made the whole batch &amp; used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe!</td>
      <td>5.0</td>
      <td>1</td>
      <td>False</td>
      <td>0.000004</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>50969</td>
      <td>2008-05-30</td>
      <td>['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']</td>
      <td>6</td>
      <td>['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']</td>
      <td>9</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
      <td>29782.0</td>
      <td>306168.0</td>
      <td>2008-12-31</td>
      <td>5.0</td>
      <td>This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!  \nThe photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.  \nThanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :)</td>
      <td>5.0</td>
      <td>4</td>
      <td>False</td>
      <td>0.000017</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>50969</td>
      <td>2008-05-30</td>
      <td>['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']</td>
      <td>6</td>
      <td>['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']</td>
      <td>9</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
      <td>1196280.0</td>
      <td>306168.0</td>
      <td>2009-04-13</td>
      <td>5.0</td>
      <td>I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.</td>
      <td>5.0</td>
      <td>4</td>
      <td>False</td>
      <td>0.000017</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>412 broccoli casserole</td>
      <td>306168</td>
      <td>40</td>
      <td>50969</td>
      <td>2008-05-30</td>
      <td>['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']</td>
      <td>6</td>
      <td>['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']</td>
      <td>since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008</td>
      <td>['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']</td>
      <td>9</td>
      <td>194.8</td>
      <td>20.0</td>
      <td>6.0</td>
      <td>32.0</td>
      <td>22.0</td>
      <td>36.0</td>
      <td>3.0</td>
      <td>768828.0</td>
      <td>306168.0</td>
      <td>2013-08-02</td>
      <td>5.0</td>
      <td>Loved this.  Be sure to completely thaw the broccoli.  I didn&amp;#039;t and it didn&amp;#039;t get done in time specified.  Just cooked it a little longer though and it was perfect.  Thanks Chef.</td>
      <td>5.0</td>
      <td>4</td>
      <td>False</td>
      <td>0.000017</td>
      <td>False</td>
    </tr>
  </tbody>
</table>


### Univariate Analysis

For this analysis, we examined the distribution of minutes in a recipe.
Many recipes are under 60 minutes to make. However, there exist outliers where it takes many thousands of minutes to make.

<iframe
  src="assets/uni_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since we care about popularity, it would be good to evaluate where or not it is actually infamous.
From the Ratings distribution, we can see that it is skewed with many people rating a 5. 
It is too be noted that stronger emotions are likely associated with a 1 than a 2.

<iframe
  src="assets/uni_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since we care about how long it takes to make the recipe, why not also pay attention to the complexity.
The distribution of the number of steps, while it doesn't imply difficulty, is nearly bell shaped but is skewed.

<iframe
  src="assets/uni_n_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

The scatter plot of interactions vs minutes shows us more bursts of data points that have a higher number of interactions closer to 0 minutes.

<iframe
  src="assets/bi_interactions_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Aggregates

We investigated the relationship between the average rating and the proportion of interactions for both minutes<=30 and minutes>30.
Visually they were not greatly different.

Tail of Pivot Table:

print(pivot.tail().to_html())

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>isduration30</th>
      <th>0.0</th>
      <th>1.0</th>
    </tr>
    <tr>
      <th>ave_rating</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4.973684</th>
      <td>0.000171</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4.977273</th>
      <td>0.000200</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4.981132</th>
      <td>0.000247</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4.990783</th>
      <td>0.000926</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5.000000</th>
      <td>0.224423</td>
      <td>0.201823</td>
    </tr>
  </tbody>
</table>


However, the sum of number of steps by average rating and sum of saturated fats by average rating for each duration group had a relatively similar shape.

<iframe
  src="assets/pivot_n_steps_ave_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/pivot_saturated_fat_ave_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness

The columns where the data experienced missing values in noticeable effect were description (114), rating (15036), and review (58).

### NMAR Analysis

Not missing at random is the chance that the evalue is missing depends on the actual missin value.
  - 'rating' could potentially be 'nmar' because giving a definitive numeric number requires a bit more effort and those who don't have a strong opinion or motivation might not be as inclined to input the data.

### Missingness Dependency

#### Future analyses will use a difference in means for test statistic and an alpha of 0.01.

We are investigating whether the missingness in the 'rating' column depends on the the column, 'saturated_fat_(PDV)', a healthiness indicator. 


> Saturated Fat

<iframe
  src="assets/missingness_saturated_fat_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


**Null Hypotheisis**: The missingness of ratings does not depend on the saturated fat (PDV) in the recipe.
**Alternative Hypothesis**: The missingness of ratings does depend on the saturated fat (PDV) in the recipe.

<iframe
  src="assets/missing_emperical_saturated_fat_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We ran a permutation test by shuffling for 500 times, and reject the null hypothesis.


We are investigating whether the missingness in the 'rating' column depends on the the column, 'minutes', variable this project is deeply interested in.

> Minutes

<iframe
  src="assets/missingness_minutes_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


**Null Hypotheisis**: The missingness of ratings does not depend on the duration of the recipe.
**Alternative Hypothesis**: The missingness of ratings does depend on the duration of the recipe.


<iframe
  src="assets/missing_emperical_minutes_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We ran a permutation test by shuffling for 500 times, and fail to reject the null hypothesis.
  - p_value of 0.148 > alpha of 0.01

#### We find that the missingness of 'rating' does depend on 'saturated_fat_(PDV)' but does not depend on 'minutes'.

## Hypothesis Testing

We are curious towards the relationship between the cooking time length and the recipe popularity.

To investigate this, we ran a **permutation test** with the following:

**Null Hypothesis**: Recipe duration labels (under or at 30 min / over 30 min) have no relationship to number of interactions.

**Alt Hypothesis**: The number of interactions of recipes m<=30 minutes and m>30 minutes come from different population distributions.

**Test Statistic**: The difference in mean between number of interations of m<=30 and m>30 recipes.

**Significance Level**: 0.01

The permutation test is useful in evaluating whether two distributions look like they came from the same population.

We propose that **people are more willing to interact with quicker to make recipes** because the shorter the time investment may make people less discouraged to get started in the kitchen.
This is only an assumption as to a possible reasoning, not a definitely tested one.

<iframe
  src="assets/hypothesis_emperical_interactions_isduration30.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We shuffle the interactions column and simulate 1000 permutations to append each test statistic to the list. 
  - float(np.mean(diff >= obs_stat)): 0.0 # we reject the null hypothesis
  - It is very possible that the quicker to make recipes and the longer to make reipes look like they come from different distributions regarding the number of interactions they receive. 

## Framing a Prediction Problem

We plan to follow a **classification problem**.

Using the average rating of the recipe, we then created 'stars' which is the average rating rounded. 

We made sure that our data set did not contain any null values for 'stars'.
  - A null value indicated that it was a recipe that never had a reviewer give it a rating. A result of the left merge or missingness of rating.

Due to the availability of resources, we just included our three metrics:
  - the accuracy,
  - the precision,
  - and the F1 score which attempts to balance accuracy and precision.

The more information, the more we can learn about how bias and 

The classification model intends to use the created features to predict the categorical star rating of (1.0, 2.0, 3.0, 4.0, 5.0).
  - After all, what's the use of having a high engagement rate other than monetary gains if the intention isn't also to share a good recipe.

## Baseline Model

For our baseline model, we are splitting the data into training and test sets. Our X variable contains data from the columns 'minutes', n_steps', 'saturated_fat_(PDV)'.
  - These variables should be familiar since they were looked at in previous analyses whether in brief or detail.

Thanks to our prior hypothesis testing, we thought it would be interesting to binarize minutes at threshold of 30 minutes.

The features were originally all quantitative until we basically transformed minutes into an ordinal feature.

The quantitative features were left as is.

Ending Model Features:
  - Quantitative: 2; Ordinal: 1; Nominal: 0

We fit a RandomForestClassifier with an undisclosed set number of n_estimators and random state.

Our model achieved the metrics:
  - accuracy: 0.7640771502080708,
  - precision: 0.5321544392688934,
  - F1: 0.3524601940747429

Being that both our training and testing accuracy were under 0.8 and a low F1 score under 0.4, the current model is not good and has much room for improvement. 

We admit that both in the base and final models was the **missed opportunity** to use k-fold cross validation to estimate the models' ability to generalize.
  - We had instead relied on eyeballing the ability to generalize by looking at the accuracy of the training set and the accuracy of the test set.


## Final Model


For the final model, we used 'minutes', 'n_steps', 'calories', 'sugar_(PDV)', 'saturated_fat_(PDV)'] which are two more features than our previous model.

The way I chose my features was calculating the r coefficient score between quantitative columns and 'rating'. 
  - By eye, I chose a few showing a stronger relationship. Then, I further narrowed it down according to personal preference.

Time and Difficulty To Execute Recipes

  - **minutes**: we have been studying the relationship between duration and rating all this while
  - **n_steps**: people are not as inclined to rate even a good recipe highly if they felt bogged down by the number of steps

Health and Taste Effects of Recipe

  - **calories**: it's the biggest number on the nutrition label and people have been trained to look for it
  - **sugar_(PDV)**: it plays a crucial role in the recipe's texture and sweetness but too much could be a health risk
  - **saturated_fat**: important source of energy but also has health risk of clogging arteries

We avoided using all possile features due to potential overfitting problems.
  - This time, instead of a binarizer, we used an InterQuartile Range Scaler to deal with minutes many estranged data points.
  - The calories feature was robustly scaled with the intention of minimizig outlier effects.
  - Other quantitative variables were just passthroughed into the RandomForestClasifer.

We performed optimization on the 'clf__max_depth' hyperparameter because having too low of a depth could result in underfitting while too high would overfit.
  - The GridSearchCV() helped us to find the max_depth just right for our clf model.

As for n_estimators, I only tried to manually optimize it between two possible values.
  - Computation time for model fitting is significant, and I was already satisfied with my results.

Our final model definitely improved from our base model.
  -  Accuracy and precision reached over the 0.9 threshold.
  -  Recall had increased by 0.34 but was still just above the 0.6 threshold.
  -  Our F1 score increased to over the 0.7 threshold.

The precision and recall are important beause the data distribution was skewed, people were more likely to give high ratings.

Our results reveal that our model returns more revelant ratings than irrelevant ones but it's also not returning as many relevant ratings as it could have.
  - At a higher level of engineering the model, there will likely have to be decisions made regarding the tradeoff.

## Fairness Analysis

For our fairness analysis, we split recipes intwo two groups: high in saturated fat and low in saturated fat.

We designated high to be a saturated fat more than 10% of the recipe's calories and low to be a saturated fat equal to or less than 10% of the recipe's calories.

Because the health recommendation is to for saturated fat <= 10% total calories, high saturated fat and low saturated fat are not ok and ok, respectively 
  - The bool column 'saturated_fat_ok' is created to differentiate between recipes above and below the recommended level in saturated fat.

Group X: 'saturated_fat_ok'

Group Y: 'stars' from our clf model of acutal and predicted rating


**Null Hypothesis**: Our model is fair. Its precision for recipes that have or don't have recommended level of saturated fats are rouphly the same, and any differences are due to random chance.

**Alt Hypothesis**: Our model is unfair. Its precision for recipes that limit to recommended saturated fats is lower than its precision for recipes that did not reach limit down to recommended saturated fats.

**Test Statistic**: Difference in precision (saturated fats ok - not ok)

**Significance Level**: 0.01

<iframe
  src="assets/fairness_emperical_precision_saturated_fat_ok.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Running a permutation test of 500 simulations, we fail to reject the null hypothesis at a p_value of 0.968.
  - Indicating that our model is likely fair and achieved precision parity regarding saturated fats with the above manipulations.

