# 🍲 Recipe Recommender System using PySpark on AWS EC2

## 📑 Project Overview
Designing a recommendation engine for **food.com** to suggest relevant recipes based on user preferences and current recipe views. The goal is to boost user engagement and increase business opportunities. This project focuses on **Exploratory Data Analysis (EDA)** using **PySpark** on **AWS EC2**.

# Recipe Recommendation System :hamburger: :pizza:

### Overview
Home cooks everywhere are always look for inspiration for new dishes to make. Searching through google helps, but google search queries return better results for just food categories (e.g. italian, japanese, baking, sauces, etc.). This recipe recommendation system intends to refine the recipe search process by allowing users to search recipes based on specific ingredients that they have or intend to use. The primary advantage for this search method is the ability to find recipes that incorporate disparate or unlikely ingredient pairings, allowing adventurous home cooks to find the cooking inspiration they otherwise couldn't find.

### Project Data
Data was pulled from github account rtlee9 who prescraped data from Allrecipes.com, Foodnetwork.com, and Epicurious.com. ~12500 recipes are included, including the title, ingredients, and instructions for each recipe.

### Goals:
1. Perform unsupervised Topic Modeling on the recipes to group recipes into categories. Then perform TextRank on the generated topics to produce keyword summarizations that can stand in as category names for each topic.
2. Create a search algorithm that utilizes similarity scoring to rank recipes according to the greatest similarity to the search query.

![Data Model Flow Chart](RecipeDataFlowChart.png)

The general flow of data is visually summarized above. Starting with the raw recipe data, the recipes are cleaned, parsed, and TF-IDF vectorized by recipe. Using this vectorization, NMF topic modelling arbitrarily generates 50 topics. TextRank then pulls 25 keywords from each topic, and these keywords are appended to the top 2000 recipes in each topic. Recipe titles, ingredients, keywords, and instructions are then TF-IDF transformed along with an ingredients list, then a query similarity score is generated for each recipe and the recipes are returned based on similarity rank.

### Data Features:
- Recipe Title
- Recipe Ingredients
- Recipe Instructions
- Recipe Keywords (Generated from Topic Modeling and Textrank)

### Model Features:
- Search based on ingredients list \(e.g. \[ingredient 1, ingredient 2, ingredient 3\]\).
- Option to rank ingredients in order of ingredients. i.e. each successive ingredient in list is weighted incrementally less in the search query.
- Generated topic keywords for a select number of recipes helps refine recipe results.

### Search Performance and Detailed Parameter Selection
For more detailed analysis of the search performance please reference the presentation slides and ipython notebook included in the repo. Included in the presentation slides are the various model parameters and the model tuning and evaluation process.

### Summary of Conclusions
While this search algorithm works quite well, it unfortuantely underutilizes the generated keywords produced using topic modelling and TextRank. The search engine penalizes recipes without any appended keywords and the tuning process neglects to include around 10,000 recipes from receiving keywords. Also, because dense word embeddings were not used, it is required for keywords to be included in the search query for there to be any similarity to the keywords at all; a better model would to generate keywords using dense word embeddings so that search query ingredients can load similarity scores based on a semantic similarity.

That being said, the cosine similarity scoring does return intriguing and useful recipes that fit the original purpose of the search algorithm. Different ingredients can be paired to find interesting and tasty recipes that one wouldn't normally encounter.

## 📂 Dataset Information
- **Recipes Data**: [RAW_recipes_cleaned.csv](https://raw-recipes-clean-upgrad.s3.amazonaws.com/RAW_recipes_cleaned.csv)
- **User Interactions Data**: [RAW_interactions_cleaned.csv](https://raw-interactions-upgrad.s3.amazonaws.com/RAW_interactions_cleaned.csv)

---

## 🚀 Project Workflow
1. **Environment Setup**: Configure AWS EC2 & install PySpark.
2. **Data Exploration**: Load and preprocess data using PySpark.
3. **EDA & Feature Engineering**: Analyze data patterns to prepare features for model building.


## 🛠️ Setup Instructions

### Step 1: Set Up EC2 Instance
```bash
ssh -i "your-key.pem" ec2-user@your-public-ip
```

### Step 2: Install Dependencies
```bash
sudo yum update -y
sudo yum install java-1.8.0-openjdk -y
pip3 install pyspark pandas matplotlib seaborn
```

### Step 3: Download Datasets
```bash
wget https://raw-recipes-clean-upgrad.s3.amazonaws.com/RAW_recipes_cleaned.csv
wget https://raw-interactions-upgrad.s3.amazonaws.com/RAW_interactions_cleaned.csv
```

## 📊 Exploratory Data Analysis
- **Load Data**:
  ```python
  from pyspark.sql import SparkSession
  spark = SparkSession.builder.appName('Recipe Recommender').getOrCreate()
  recipes_df = spark.read.csv('RAW_recipes_cleaned.csv', header=True, inferSchema=True)
  interactions_df = spark.read.csv('RAW_interactions_cleaned.csv', header=True, inferSchema=True)
  ```
- **Visualize Ratings Distribution**:
  ```python
  import seaborn as sns
  sns.histplot(interactions_pd['rating'], bins=5)
  ```

## 🔍 Key Insights
- Recipes with higher ratings attract more interactions.
- Seasonal trends observed for certain recipe categories.

---

## 📌 Future Enhancements
- Implement collaborative filtering models.
- Deploy using AWS SageMaker.
