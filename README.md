## The Relationship Between Recipe Ratings and the Time They Take
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

I also made sure to handle extreme values. Some recipes had unrealistic preparation time. I removed these outliers in order to ensure that my data would not be abnormally skewed.

Below is the head of my cleaned DataFrame with key features shown:

| id | minutes | n_steps | avg_rating | calories | n_tags |
| --- | ------ | ------- | ---------- | -------- | ------ |
|333281 | 40 | 10 | 4.0	| 138.4	| 219 |
| 453467 |	45	|	12	|	5.0	| 595.1	| 157 |
| 306168 |	40	|	6	|	5.0	| 194.8	| 148 |
| 286009	| 120 |	7	|	5.0 |	878.3 |	290 |
|	475785 |	90	|	17	|	5.0	| 267.0	| 150 |

#### **Univariate Analysis**

<iframe
  src="assets/rating_fig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### **Assessment of Missingness**

### **Hypothesis Testing**

### **Framing a Prediction Problem**

### **Baseline Model**

### **Final Model**

### **Fairness Analysis**
