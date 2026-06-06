## The Recipe to a Good Rating
By: Casee Hong
### **Introduction**
The dataset I am using is the Recipe and Ratings dataset which features 83,782 unique recipes and 731,927 user ratings. The recipes contain attributes such as the time it takes to complete them, the number of steps, ingredients, and nutritional value through factors like calories and protein. This data offers the opportunity to analyze and draw connections to ultimately dsicover what makes a recipe "good" to users who try them.
#### **Central Question**
**What is the realtionship between the cooking time and average rating of recipes?**

This question is important in understanding user preferences and helping cooks develop and post recipes that are well-recieved by the public. Cooking time is a vital metric that can often be a deciding factor in choosing which recipes to use because it comes with the implications of efficiency, optimization, accessibility, complexity, and convenience. This question will help us discover if users tend to prefer quick and simple recipes or longer recipes with more complex flavor profiles.

Additionally, this project will also look at other contributing factors such as the number of steps (which further measures complexity and how descriptive instructions are), the number of tags (a feature that measures a recipe's reach), and calories (a nutritional value that can uncover how filling the recipes's product is) to see what gives recipes higher ratings. By looking at a variety of key factors, we can explore their combined influence on user ratings to answer the broader question of what makes a recipe highly rated.
#### **Relevant Columns**
This dataset contains CSV files RAW_recipes.csv and interactions.csv -- these files have a variety of rows that provide unique information. These are the ones that are relevant to my question:
- **minutes**: Minutes needed to prepare the recipe
- **tags**: List of tags attached to the recipe
- **nutrition**: Nutrition information in the form of a string that looks like [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”
- **n_steps**: Number of steps in the recipe
- **rating**: Rating given by an individual user


### **Data Cleaning and Exploratory Data Analysis**
#### **Data Cleaning**
The two given CSV files were merged on the 'recipe_id' column in order to create one recipes dataframe. With that, there were a large number of columns and cleaning that had to be done within some of those columns. 

The first column I tackled was the 'nutrition' column. In order to get the numerical values and separate them into separate categories for each given nutritional value, I first stripped the column values of their brackets and split the numerical values by ', ' in order to create a list of strings of numbers rather than one long string. I then used this list and the .apply function to convert these values into floats and gave each nutritional category their own column -- 'calories', 'total_fat', 'sugar', 'sodium', 'protein', 'saturated_fat', and 'carbohydrates'.


I decided to add two new columns that I felt were important to my question: 'avg_rating' and 'n_tags':

- 'avg_rating' features each recipe's average rating. This column was created by grouping the merged recipes dataframe by 'id' and finding the mean of the grouped ratings before mapping the results onto the recipes dataframe 'id' column. This additional column is important in discovering general user consensus about each recipe.
- 'n_tags' displays the number of tags each recipe has. This was created using the .apply function and finding the length of the list of tags in the 'tags' column. This additional column lets us examine whether or not the number of tags on a recipe play a role in user ratings.

Another thing I did was drop duplicates of the id column. The merged DataFrame had duplicate recipes because of the different user interactions for each recipe. This resulted in repeated unchanging values and statistics that could skew the data.

I also made sure to handle extreme values. Some recipes had unrealistic preparation time. I removed these outliers by dropping the rows containing them in order to ensure that my data would not be abnormally skewed.

Below is the head of my cleaned DataFrame with key features shown:

| id | minutes | n_steps | avg_rating | calories | n_tags |
| --- | ------ | ------- | ---------- | -------- | ------ |
|333281 | 40 | 10 | 4.0	| 138.4	| 219 |
| 453467 |	45	|	12	|	5.0	| 595.1	| 157 |
| 306168 |	40	|	6	|	5.0	| 194.8	| 148 |
| 286009	| 120 |	7	|	5.0 |	878.3 |	290 |
|	475785 |	90	|	17	|	5.0	| 267.0	| 150 |

#### **Univariate Analysis**
This histogram displays the distribution of average ratings. The data is left-skewed with most recipes having average ratings that fall in between 4.75 and 5 stars. Low and moderate ratings are relatively uncommon.
<iframe
  src="assets/rating_fig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### **Bivariate Analysis**
This scatter plot shows the relationship between average ratings and cooking time in minutes. The results show that recipes under 50 minutes vary greatly between moderate and high ratings with many recipes falling in that cooking time range. Around the range of 50-100 minutes is when recipes seem to have ratings that are more consistently high.
<iframe
  src="assets/avg_vs_mins.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

#### **Interesting Aggregates**

| Cooking Time Bin | Average Rating |
| ----------- | -------------- |
| 0-15 minutes | 4.55 |
| 15-30 minutes | 4.51 |
| 30-60 minutes | 4.47 |
| 1-2 hours | 4.47 |
| over 2 hours | 4.45 |

Cooking time was grouped into intervals in order to examine more closely which time range tends to be the most popular. Recipes under 15 minutes performed the best with recipes in the other ranges doing slighly worse. Recipes over 2 hours performed the worst with an average rating of about 0.10 less. This tells us that users prefer quicker recipes.

### **Assessment of Missingness**
#### **NMAR Analysis**
The rating and, in turn, avg_rating columns of my DataFrame have missing data in the form of 0s. This was discovered by seeing inconsistencies in written reviews that complimented the recipe and an attached 0 star rating. Upon further inspection, it was also discovered that food.com, the website in which the data comes from, does not allow 0 star ratings and that 1 star was the lowest rating possible. I thought it could be possible that those missing ratings were a result of the recipes being newer and thus having insufficient data which is why I think the avg_rating column is NMAR (Not Missing At Random).
#### **Missingness Dependency**
A permutation test was ran where the null hypothesis is that there is no relationship between the missingness of avg_rating and submission date (date_num column created) and the alternative hypothesis is that there is a relationship between avg_rating and submission date. This resulted in a p-value of about 0.0009 which is less than 0.05, allowing us to reject the null and assume the missingness of avg_rating has to do with the recipe's submission date.

This plot shows that the distribution of dates features a large chunk of later, or more recent, dates that do not appear when there are no missing ratings.
<iframe
  src="assets/date_missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

In order to not skew statistics and data, I replaced the 0 values in avg_rating with the overall mean of avg_rating. I tested the missingness of avg_rating against other columns in the DataFrame and found that most columns do have some influence in the missingness of average ratings. Alternatively, the description column which also had missing values, is *not* NMAR, and is instead MCAR which was revealed to me when I tested its missingness against the column 'calories' and got a p-value of about 0.2477. To remedy this type of missingness, I simply filled the NaN values with "No description" to resolve the columnn of null values while keeping values strings.

### **Hypothesis Testing**
A separate permutation test was ran to see if there is truly a relationship between the cooking time of a recipe and its average ratings. 
- **Null hypothesis**: There is no relationship between the cooking time of a recipe and its average ratings.
- **Alternative hypothesis**: The cooking time of a recipe influences its average ratings.
- **Test statistic**: Correlation coefficient
- **Significance level**: 0.05
- **Resulting p-value**: 0.0009

I chose this pair of hypotheses to help me answer my central question: What is the realtionship between the cooking time and average rating of recipes? I decided to use correlation coefficient as my test statistic because 'minutes' is a quantitative column and avg_rating is a column that we can treat as quantitative. Because I both columns can be treated as quantitative and I'm trying to uncover a possible relationship between the two, correlation coefficient is a good test statistic. I chose to use the typical significance level of 0.05 as to not make my hypothesis too strict, but also ensure some level of accuracy. The resulting p-value was 0.0009 which is less than my test statistic, signifying that I can reject my null hypothesis and assume that there is a relationship between the cooking time of a recipe and its average rating.

### **Framing a Prediction Problem**
#### **Problem Identification**
I wanted to predict a recipe's average rating. This is a regression type problem. I chose to predict this column because I feel like it provides valuable information as the response variable. By using key information provided by a recipe such as cooking time, number of steps, number of tags, and number of calories, we can predict how users will respond to the recipe before they even rate it or before the recipe gains sufficient enough feedback to create an accurate average rating. This can be used to push recipes that users are likely to rate highly or even help recipe developers find and develop recipes that more people will enjoy. I chose to use MSE (Mean Squared Error) as my evaluation metric because it closely measures and penalizes larger errors which makes it a good metric to measure the differences between predicted and actual values.

### **Baseline Model**
My baseline model uses four quantitative features:
- **'minutes'**: The cooking time of the recipe in minutes. This measures how long a recipe takes.
- **'n_steps'**: The number of steps in the recipe. This can measure recipe complexity and simplicity.
- **'n_tags'**: The number of tags attached to the recipe. This can determine a recipe's reach and popularity through categorization.
- **'calories'**: An important nutritional value that people tend to look at.

These values are all available at prediction time and may relate to a recipe's ratings. Since all of these features are numeric, there was no need for categorical encoding. I trained a linear regression model using an sklearn Pipeline and evaluated performance on a held-out test set. The mean squared error of this model is about 0.5654 -- this model has decent performance, but it has room for improvement. I believe this model is a good foundation because of its substantial performance, but it has a lot of room for improvement through things like feature engineering and fine-tuning.

### **Final Model**
In order to improve my final model, I made a few changes:
#### **Feature Engineering**
I engineered two new features:
- **'minutes_per_step'**: This measures on average how complex the steps in a recipe are. A 60 minute recipe with 42 steps is very different from a 60 minute recipe with 5 steps.
- **'calories_per_step'**: This measures the average amount of calories gained per step in a recipe. This can tell us a recipe's richness or density relative to its complexity.

I believe these new features improve my model's performance because they measure nuances that cannot be gathered through the non-engineered data. These two features help us make more accurate assumptions about a recipe's complexity which is likely to impact its ratings.
#### **Switching Models**
I decided to switch my model to RandomForestRegressor due to its ability to capture nonlinear relationships. This better fits my data and improves my model's accuracy.
#### **Hyperparamaters**
I tuned the maximum depth of the trees (max_depth) because deeper trees can capture more complex relationships but may also overfit the training data. I also tuned the number of trees in the forest (n_estimators) because larger forests generally result in more stable predictions, though they also have increased computation. Hyperparameters were selected using GridSearchCV to minimize mean squared error.
#### **Final Results**
My final model resulted in an MSE of about 0.5643 which is a slight improvement from my baseline model. Upon selecting hyperparameters for this model, I noticed that the best max_depth was only 5; this suggests that a relatively simple model generalized best and concludes that recipe ratings may not be strongly determined by cooking time, number of steps, number of tags, or calories alone.

### **Fairness Analysis**
In order to see if my model is fair, I chose to group my data based on recipe cooking times that are under 40 minutes vs cooking that are greater than or equal to 40 minutes. I chose this grouping because it seemed that most recipes tended to fall under 40 minutes. For my permutation test I used:
- **Null hypothesis**: This model does not performs equally well for recipes with cooking times under 40 minutes and recipes with cooking times equal to or over 40 minutes.
- **Alternative hypothesis**: This model performs differently for recipes with cooking times under 40 minutes and recipes with cooking times equal to or over 40 minutes.
- **Test statistic**: RMSE (Root Mean Squared Error)
- **Significance level**: 0.05
- **Resulting p-value**: 0.0009

With the resulting p-value of 0.0009 being less than the significance level of 0.05, we can reject the null hypothesis and assume that this model performs differently based on long vs. shorter cooking times. This indicated that the model may potentially have some bias or uneven prediction performance.
