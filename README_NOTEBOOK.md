# H&M Customer Segmentation Analysis - Code Notebook

## 📋 Overview

This Jupyter notebook implements a complete customer segmentation pipeline for H&M fashion retail data using K-Means clustering. The project processes raw transaction, customer, and article data to identify distinct customer segments based on behavioral and demographic characteristics, enabling targeted marketing strategies.

**Dataset**: H&M Personalized Fashion Data (Kaggle)

---

## 🎯 Project Objectives

1. **Data Collection**: Download and extract H&M fashion data using Kaggle API
2. **Data Quality**: Audit raw data for completeness and integrity
3. **Exploratory Analysis**: Understand patterns in transactions, customers, and products
4. **Data Cleaning**: Prepare clean datasets for analysis
5. **Feature Engineering**: Create RFM (Recency, Frequency, Monetary) and behavioral features
6. **Feature Selection**: Identify optimal features through variance and correlation analysis
7. **Clustering**: Apply K-Means to segment customers into 5 distinct groups
8. **Insights**: Generate actionable business recommendations for each segment

---

## 📂 Project Structure

### Data Directories

- **`data/raw/`**: Original datasets from Kaggle
  - `transactions_hm.csv` - Transaction-level purchase data
  - `customer_hm.csv` - Customer demographics and preferences
  - `articles_hm.csv` - Product/article information

- **`data/cleaned/`**: Cleaned and preprocessed data
  - `transactions.csv` - Cleaned transactions (no missing values)
  - `customers.csv` - Cleaned customer records (selected features)
  - `articles.csv` - Cleaned articles (selected features)

- **`data/04_features/`**: Feature engineering outputs
  - `features.csv` - All engineered features
  - `features_filtered.csv` - Features after correlation and variance filtering
  - `scaled_final_features_for_clustering.csv` - Normalized features ready for clustering

- **`data/05_clustered/`**: Clustering results
  - `customer_segments.csv` - Final customer segments with cluster labels
  - `cluster_centers.csv` - K-Means cluster centers
  - `cluster_summary_for_marketing.csv` - Business summary for marketing teams

### Configuration

- **`kaggle_config/`**: Kaggle API configuration
  - `kaggle.json` - API credentials for data download

---

## 🔄 Processing Pipeline

### 1. Data Extraction (Cells 1-8)
- Install and configure Kaggle API client
- Set up Kaggle authentication using API token
- Download H&M fashion data via Kaggle CLI
- Extract ZIP file to `data/raw/` directory

### 2. Data Audit & Availability Check (Cells 9-10)
- Validate data structure and completeness
- Identify missing values per column
- Generate descriptive statistics for numerical fields
- Check categorical distributions
- Confirm key field uniqueness (e.g., article_id, customer_id)

### 3. Exploratory Data Analysis (Cells 11-12)
Comprehensive EDA using reusable functions:
- **basic_eda()**: Shape, dtypes, memory usage, sample data
- **analyze_missing_values()**: Missing data counts and heatmaps
- **analyze_numerical()**: Distributions, box plots, skewness
- **analyze_categorical()**: Value counts, proportions, bar charts
- **analyze_correlations()**: Correlation matrices and high-correlation detection
- **analyze_timeseries()**: Time-based patterns (daily/monthly aggregations)

### 4. Data Cleaning (Cells 13-14)
- **Transactions**: Convert date format, remove rows with any missing values
  - Result: ~31.4M transactions (1.3M removed with missing data)
- **Articles**: Filter to key columns (`article_id`, `index_group_name`), remove nulls
  - Result: 105,542 articles (no removals)
- **Customers**: Select key demographics, drop rows with missing values
  - Result: ~1.4M customers (0.7M removed with missing data)

### 5. Feature Engineering (Cells 15-16)
Creates three feature categories:

**A. RFM Features** (from transactions):
- **Recency**: Days since last purchase
- **Frequency**: Number of unique transaction dates (orders)
- **Monetary**: Total spend in euros
- **Total_Items**: Number of articles purchased
- **Avg_Channel**: Sales channel preference (1=online, 2=store)
- **Avg_Order_Value**: Total spend / frequency
- **Avg_Item_Price**: Total spend / item count

**B. Behavioral Features** (product preferences):
- Top 5 product categories by percentage of customer purchases
- Captures fashion segment preference (e.g., Menswear, Ladieswear, Accessories)

**C. Demographic Features** (customer attributes):
- **age**: Customer age (median imputation for missing values)
- **is_newsletter_active**: Binary indicator (Active status)
- **fashion_news_frequency**: One-hot encoded (Regularly subscribed: 0/1)

### 6. Feature Selection (Cell 17)
- **Scale numerical features** using StandardScaler
- **Remove low-variance features** (threshold < 0.01)
- **Remove highly correlated features** (correlation > 0.9)
- Typical result: ~18-22 features retained from initial 20+

### 7. Clustering (Cells 18-30)
**K-Means Implementation**:
- **Optimal k determination**: Elbow method + Silhouette score analysis
- **Selected k**: 5 clusters (based on problem statement)
- **Initialization**: n_init=10 for stability
- **Random state**: 42 for reproducibility

**Cluster Analysis**:
- Distribution of customers across clusters
- Cluster characteristics (mean feature values)
- Normalized heatmap comparison
- Radar charts for visual segment profiles
- Business interpretations and strategies

---

## 📊 Key Features

### RFM Metrics
- **Recency (R)**: How recently did customer purchase? (measured in days)
- **Frequency (F)**: How often does customer purchase? (transaction count)
- **Monetary (M)**: How much does customer spend? (total euros)

These classic RFM metrics are crucial for identifying customer value and engagement patterns.

### Feature Scaling
- StandardScaler applied to numerical features before clustering
- Ensures features with large ranges (e.g., Monetary) don't dominate distance calculations

### Behavioral Segmentation
Categories analyzed:
- **Product Preferences**: Top fashion segments (Menswear, Ladieswear, etc.)
- **Purchase Patterns**: Order frequency and average transaction value
- **Engagement**: Newsletter subscription and channel preference

---

## 🎭 Customer Segments Identified

### Typical Cluster Profiles

| Cluster | Name | Characteristics | Strategy |
|---------|------|-----------------|----------|
| 0 | VIP/High-Value | Low recency, high frequency, high spend | Premium service, exclusive offers |
| 1 | Core Loyal | Recent purchases, consistent behavior | Retention programs, loyalty rewards |
| 2 | At-Risk | High recency, low frequency/spend | Re-engagement campaigns, win-back offers |
| 3 | New/Dormant | Low purchase history | Onboarding, product discovery |
| 4 | Active Medium-Value | Moderate metrics across all dimensions | Growth programs, cross-sell/upsell |

*Actual cluster names and strategies generated dynamically based on data patterns*

---

## 📈 Outputs Generated

### CSV Outputs
1. **customer_segments.csv** - Customer IDs with cluster assignments
2. **cluster_centers.csv** - Feature values at cluster centers
3. **cluster_summary_for_marketing.csv** - Business-friendly summary table

### Visualizations
- **Elbow curves** for optimal k selection
- **Silhouette score plots** for cluster quality
- **Cluster distribution** (bar chart & pie chart)
- **Cluster center heatmap** (feature comparison)
- **Radar charts** (cluster profiles)
- **Distribution plots** (price, age, etc.)

---

## 🛠️ Technologies Used

### Python Libraries
- **Data Processing**: pandas, numpy
- **Visualization**: matplotlib, seaborn
- **Machine Learning**: scikit-learn (KMeans, StandardScaler, PCA)
- **API Integration**: kaggle
- **Utilities**: zipfile, os

### Key Modules
- **sklearn.cluster.KMeans** - Customer segmentation
- **sklearn.preprocessing.StandardScaler** - Feature normalization
- **sklearn.metrics.silhouette_score** - Cluster quality metric
- **sklearn.decomposition.PCA** - Dimensionality reduction (available)

---

## 📖 Usage Instructions

### Prerequisites
1. Python 3.7+ with Jupyter Notebook
2. Kaggle API credentials (`kaggle.json`)
3. Required libraries: `pip install kaggle pandas numpy scikit-learn matplotlib seaborn`

### Running the Notebook

1. **Setup Kaggle API**:
   - Place `kaggle.json` in `kaggle_config/` directory
   - Ensure proper permissions: `chmod 600 kaggle_config/kaggle.json`

2. **Execute Sequentially**:
   - Run cells 1-8: Download data
   - Run cells 9-10: Data audit
   - Run cells 11-12: EDA
   - Run cells 13-14: Data cleaning
   - Run cells 15-16: Feature engineering
   - Run cell 17: Feature selection
   - Run cells 18-30: Clustering and analysis

3. **View Results**:
   - Check `data/` directory for CSV outputs
   - Review generated visualizations in notebook cells
   - Read cluster summaries for business insights

---

## 🔍 Key Findings Areas

### Cluster Quality Metrics
- **Silhouette Score**: Measures how similar customers are within clusters
- **Inertia**: Sum of squared distances from cluster centers
- **Cluster Balance**: Distribution of customers across segments

### Feature Importance
- **Recency** dominates segment separation (recent vs. dormant)
- **Monetary value** identifies high-value customers
- **Age demographics** shows engagement patterns by customer age group
- **Newsletter subscription** correlates with engagement

### Business Insights
- VIP customers (high frequency/monetary, low recency) warrant premium treatment
- At-risk customers (high recency) need targeted re-engagement
- New customers benefit from onboarding and discovery programs
- Product category preferences vary significantly by segment

---

## 📝 Notes & Considerations

### Data Handling
- Missing values handled via row removal or mean imputation (context-dependent)
- Customer IDs preserved throughout pipeline for result traceability
- Date format standardized to datetime for time-series analysis

### Feature Engineering Choices
- RFM metrics chosen for proven effectiveness in retail segmentation
- Product category preferences limited to top 5 to avoid sparse features
- Age normalized for clustering alongside categorical features

### Clustering Parameters
- **Random state = 42**: Ensures reproducible results
- **n_init = 10**: Multiple initializations for stable solutions
- **k = 5**: Problem-specified number of segments

### Potential Improvements
- Try hierarchical clustering or DBSCAN for non-spherical segments
- Implement PCA for high-dimensional visualization
- Add temporal features (seasonal patterns)
- Use silhouette analysis for k optimization
- Implement customer lifetime value (CLV) prediction

---

## 🎓 Educational Value

This notebook demonstrates:
- Complete ML pipeline from data collection to insights
- RFM analysis in retail context
- Feature engineering best practices
- Clustering evaluation metrics
- Business translation of technical results
- Code organization with reusable functions

---

## 📞 Contact & Support

For questions about the analysis methodology or results interpretation:
- Review inline comments and markdown explanations
- Consult cluster_summary_for_marketing.csv for business context
- Check data quality reports in audit cells

---

## 📄 License & Attribution

**Dataset**: H&M Personalized Fashion Data
**Source**: Kaggle (https://www.kaggle.com/datasets/sohyunjun0401/h-and-m-personalized-fashion-data)

This notebook is part of the APEX Project for customer intelligence analysis.

---

## 🚀 Version History

- **v1.0** (November 2025): Initial complete pipeline
  - Data collection via Kaggle API
  - Comprehensive EDA functions
  - RFM feature engineering
  - 5-cluster K-Means segmentation
  - Business-friendly cluster interpretation

---

**Last Updated**: November 2025  
**Status**: Production Ready ✅
