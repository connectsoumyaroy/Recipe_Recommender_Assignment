Here's the summarized GitHub `README.md` in Markdown format:

```markdown
# 🍲 Recipe Recommender System using PySpark on AWS EC2

## 📑 Project Overview
Designing a recommendation engine for **food.com** to suggest relevant recipes based on user preferences and current recipe views. The focus is on **Exploratory Data Analysis (EDA)** using **PySpark** on **AWS EC2**.

## 📂 Dataset Information
- **Recipes Data**: [RAW_recipes_cleaned.csv](https://raw-recipes-clean-upgrad.s3.amazonaws.com/RAW_recipes_cleaned.csv)
- **User Interactions Data**: [RAW_interactions_cleaned.csv](https://raw-interactions-upgrad.s3.amazonaws.com/RAW_interactions_cleaned.csv)

---

## 🚀 Project Workflow
1. **Environment Setup**: Configure AWS EC2 & install PySpark.
2. **Data Exploration**: Load and preprocess data using PySpark.
3. **EDA & Feature Engineering**: Analyze data patterns for model building.

---

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

---

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

---

## 🔍 Key Insights
- Recipes with higher ratings attract more interactions.
- Seasonal trends observed for certain recipe categories.

---

## 📌 Future Enhancements
- Implement collaborative filtering models.
- Deploy using AWS SageMaker.

## 💻 Author
**Soumya Roy**  
[Connect on LinkedIn](https://www.linkedin.com/in/connectsoumyaroy/)

## 📜 License
Licensed under the MIT License.
```

Feel free to copy and paste this Markdown code into your `README.md` file!
