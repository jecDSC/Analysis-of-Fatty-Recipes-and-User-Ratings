# Food and Time Cost
# Overview
This project was conducted for Practice and Application of Data Science (DSC 80) at UCSD. It aims to further insight into how significant the time cost of a recipe is to people's perception of recipes and/or cooking.
# Introduction
Recipes are an important tool for any cook, whether it's for needing a step-by-step guide on a dish, learning the basics through a guide, and so much more. The taste and overall satisfaction from the resulting product of a recipe would be an average person's metrics for evaluating how good a recipe is.

But there are many who give more thought to the contents of the meals they create and eat. Some may focus specifically on several nutritional metrics in order to keep track of a healthy diet. Especially in America, where we have one of the highest obesity rates in the world, the fat content of foods can be especially worrisome. Fast-food and quick meals are widespread and convenient, but those who seek out recipes to cook may do so in order to find healthy alternatives to avoid the pitfalls of convenient, cheap meals-to-go.

Taking this mindset of some people into consideration, one might wonder **if the fat content of a recipe influences one's rating of the recipe.**

To explore this question, I will use two datasets from [food.com](food.com), one of recipes and another of ratings of recipes across the website. Merging the two datasets results in 234429 rows of data ready for analysis. More on how the two datasets were merged will be explained later.

Here are the columns of the merged dataset:
- `'name'`: The name of the recipe.
- `'id'` and `'recipe_id'`: The ID of the recipe.
- `'minutes'`: The number of minutes to prepare the recipe.
- `'contributor_id'`: The user ID of the user who submitted the recipe.
- `'submitted'`: The date the recipe was submitted to the website.
- `'tags'`: Food.com tags for the recipe.
- `'nutrition'`: Nutrition information of the recipe. [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV -> Percent Daily Value
- `'n_steps'`: The number of steps in the recipe.
- `'steps'`: The recipe steps in text.
- `'description'`: User-provided description of the recipe.
- `'user_id'`: The user ID of the user who submitted the review.
- `'date'`: The date the review was submitted.
- `'rating'`: The numeric rating given to the recipe.
- `'review'`: The text review for the recipe.

# Data Cleaning and Exploratory Data Analysis
## Data Cleaning
1. The first step is to left-merge the recipe dataset with the reviews dataset, so that each row has both the recipe details and a review of that recipe.

2. The rating column has 0.0 values for some reviews. The website food.com allows users to leave reviews without providing a numerical rating, which mean2 that these 0.0 values are not representative of a rating of 0. Rather, the 0.0 means a numerical value is missing for these reviews. Therefore, all 0.0 rating need to be  replaced with `np.nan`.

3. A column, `average_rating`, will be added. This will provide the average rating of the corresponding recipe for each row.

4. The nutrition column will be split into its components. Each value in nutrition is a list (in string form) of calorie count, total fat (Percent Daily Value), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV), respectively. This leads to seven new columns being created, with each value being a float value of the specified nutritional component.

5. A column, `more_fatty`, will be created. For each recipe, this will hold a Boolean value that indicates whether a recipe has a higher total fat content than the median or not. The reason the median is used is because the total fat column holds some extreme outliers. Using the mean would skew the data, so we will use the median to lessen the influence of those outliers.

## Univariate Analysis

Because of outliers, visualizing the minute values using a plot is not ideal, as shown below.

# Assessment of Missingness
## NMAR Analysis
In the merged DataFrame, four columns, `rating`, `review`, `date`, and `average_rating`, have missing values. Out of these, it is possible that the missingness mechanism of `review` is Not Missing At Random (NMAR). It's likely that if a review is missing, it could be due to a user not liking a recipe enough to set aside even a bit of time to write a sentence or two for a review. In addition, rating the recipe is a task that requires less effort than writing a recipe. For some, giving a recipe a rating between one and five stars may suffice to express their opinion on a recipe, rather than including a worded review to go with it.

## Missingness Dependency
We will now explore the dependency of the `rating` column on two other columns, `n_steps` and `minutes`. Each test will use a significance level of 0.05, and the test statistic will simply be the absolute difference between two groups: one that is missing `rating` data and one that is not missing `rating` data.

First, we will take a look at **if there is a dependency between `rating` and `n_steps`**.
**ADD PLOT HERE**
- **Null Hypothesis**: The missingness of `rating` **does not** depend on the number of steps.
- **Null Hypothesis**: The missingness of `rating` **does** depend on the number of steps.

Below is the plot of the simulated differences of 500 permutations of the `rating_missing` column, which indicates with a Boolean whether or not a row is missing `rating` data. The observed test statistic will also be indicated by a green line. The resulting p-value is 0.0. 

**ADD PLOT HERE LATER**

This leads us to **reject the null**. The missingness of `rating` depends on the column `n_steps`.

Next, we will take a look at **if there is a dependency between `rating` and `minutes`**.
**ADD PLOT HERE**
Note that the above plot's scale was modified to ensure a fitting representation of the data, as there are extreme outliers in the minutes column.

- **Null Hypothesis**: The missingness of `rating` **does not** depend on the length of the recipe in minutes.
- **Null Hypothesis**: The missingness of `rating` **does** depend on the length of the recipe in minutes.

The same process as above was used for this permutation test. Below is the plot of the differences and the observed test statistic as a green line. The resulting p-value is 0.118.

**ADD PLOT HERE LATER**

This leads us to **failing to reject the null**. The missingness of `rating` does not depend on the column `minutes`.
