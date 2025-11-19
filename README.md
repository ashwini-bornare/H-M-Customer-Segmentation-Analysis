# H&M Customer Segmentation Analysis

## Project Overview
This project processes and analyzes H&M transaction data to create customer segments based on purchasing behavior, preferences, and demographics. The analysis combines RFM (Recency, Frequency, Monetary) metrics with product preferences and customer attributes.

## Data Files
- `transactions.csv`: Customer purchase history
  - Key fields: customer_id, article_id, t_dat (date), price, sales_channel_id
- `customers.csv`: Customer demographic information
  - Key fields: customer_id, age, club_member_status, fashion_news_frequency, Active
- `articles.csv`: Product catalog data
  - Key fields: article_id, product details, color information, category groupings

## Workflow
1. **Data Loading & Preprocessing**
   - Load transaction, customer, and article data
   - Handle data types (dates, IDs)
   - Early merge of transactions with article metadata

2. **Feature Engineering**
   - RFM Features:
     - Recency: Days since last purchase
     - Frequency: Number of unique transaction dates
     - Monetary: Total spend and derived metrics
   - Product Preferences:
     - Product group preferences
   - Demographics:
     - Age normalization
     - Newsletter/news preferences

3. **Data Preparation for Clustering**
   - Feature matrix construction
   - Handling missing values
   - Scaling numerical features

## Requirements
```
pandas
numpy
scikit-learn
matplotlib
seaborn
```

## Usage
1. Place the CSV files in the /data
2. Run the notebook cells sequentially
3. The final output is a prepared feature matrix ready for clustering

## Output
- Engineered features dataframe with:
  - RFM metrics
  - Product category preferences
  - Demographic features
  - All features appropriately scaled/encoded