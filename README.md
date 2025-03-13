# Food and Time Cost
# Overview
This project was conducted for Practice and Application of Data Science (DSC 80) at UCSD. It aims to further insight into how significant the time cost of a recipe is to people's perception of recipes and/or cooking.
# Introduction
Recipes are an important tool for any cook, whether it's for needing a step-by-step guide on a dish, learning the basics through a guide, and so much more. The taste and overall satisfaction from the resulting product of a recipe would be an average person's metrics for evaluating how good a recipe is.

But what metrics would those who may not have the luxury of slow-cooking and the time to follow finely-detailed instructions use to rate a recipe? College students may be the perfect example of this category of people. The pressure and time constraints of homework, studies, employment or internships, and other activities, academia-related or not, are just some of the items that may make up a student's daily life.

In recent times, the concept of "meal-prep" has become widespread with the help of social media. Creating food in large quantities in a single day, then portioning that food throughout the week has become a common way for some to take care of their meals.

As more people begin to embrace this method of efficient cooking, one might wonder **if the time cost of a recipe influences one's rating of the recipe.**

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
- The first step is to left-merge the recipe dataset with the reviews dataset, so that each row has both the recipe details and a review of that recipe.

- Some more cleaning steps will be necessary to make the dataset simpler for analysis. Only the columns that may be relevant to this analysis will be kept for simplification purposes. Here are the kept columns and their datatypes.
- `'id'`: int64
- `'minutes'`: int64
- `'tags'`: str
- `'n_steps'`: int64
- `'n_ingredients'`: int64
- `'rating'`: float64

- The rating column has 0.0 values for some reviews. The website food.com allows users to leave reviews without providing a numerical rating, which means that these 0.0 values are not representative of a rating of 0. Rather, the 0.0 means a numerical value is missing for these reviews. Therefore, all 0.0 rating need to be  replaced with `np.nan`.

- A column, `average_rating`, will be added. This will provide the average rating of the corresponding recipe for each row.

- At this point, it will be fine to drop the `rating` column, since the `average_rating` column will store the rating info for each recipe, and we are not aiming to analyze this dataset based on individual reviews. After `rating` is dropped, there will be duplicate rows that exist since a recipe can have more than one review. Duplicate rows will be dropped to ensure that there is only one row of information per recipe.

## Univariate Analysis