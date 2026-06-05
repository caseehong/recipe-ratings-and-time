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

### **Assessment of Missingness**

### **Hypothesis Testing**

### **Framing a Prediction Problem**

### **Baseline Model**

### **Final Model**

### **Fairness Analysis**
