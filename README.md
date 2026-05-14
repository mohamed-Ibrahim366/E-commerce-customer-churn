# E-commerce Customer Churn Analysis

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-ML-F7931E?logo=scikit-learn)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)

---

## Project Overview

This project analyzes an **E-commerce Customer Churn dataset** containing **50,000 customer records** with **25 features** to predict and understand customer churn behavior. The analysis includes:

- **Data Cleaning & Preprocessing** – handling missing values, duplicates, and type conversions
- **Exploratory Data Analysis (EDA)** – visualizing churn patterns and customer demographics
- **Machine Learning Models** – training Logistic Regression, Random Forest, and Gradient Boosting classifiers
- **Model Evaluation** – comparing performance using cross-validation and classification metrics
- **Insights & Recommendations** – identifying key churn drivers

---

## Project Structure

```text
e-commerce_customer_churn/
├── ecommerce_customer_churn_dataset.csv   # Raw dataset (50,000 customers)
├── churn_cleaned.csv                       # Cleaned & processed data
├── customer_churn.ipynb                    # Main analysis notebook
├── docker-compose.yml                      #put the project in docker container
├── model_comparison.png                    # ML model performance comparison
├── feature_importance.png                  # Feature importance analysis
├── kfold_boxplot.png                       # Cross-validation results
├── mapreduce_chart.png                     # Additional performance metrics
├── customer_churn_results.png              # Results summary
└── README.md
```

---

## Dataset Overview

| Attribute | Details |
|---|---|
| **Source** | E-commerce Customer Churn Dataset |
| **Format** | CSV |
| **Size** | 50,000 customer records |
| **Features** | 25 columns |
| **Target Variable** | `Churned` (1 = churned, 0 = retained) |
| **Churn Rate** | ~29% |

### Key Features

| Column | Description |
|---|---|
| `Age` | Customer age |
| `Gender` | Customer gender |
| `Country` / `City` | Geographic location |
| `Total_Purchases` | Number of purchases made |
| `Cart_Abandonment_Rate` | % of abandoned shopping carts |
| `Lifetime_Value` | Total value to business |
| `Session_Duration_Avg` | Average browsing session (minutes) |
| `Days_Since_Last_Purchase` | Recency metric |
| `Churned` | Target (churn status) |

---

## Data Processing Pipeline

The analysis includes a comprehensive data cleaning workflow:

1. **Load Raw Data** – Read 50,000 customer records
2. **Remove Duplicates** – Identify and drop duplicate rows
3. **Handle Missing Values** – Fill numeric columns with median values
4. **Standardize Text Fields** – Clean gender, country, and city names
5. **Type Conversion** – Ensure correct data types
6. **Export Clean Data** – Save processed data as `churn_cleaned.csv`

---

## Machine Learning Models

The project trains and evaluates **3 classification models** using 5-fold Stratified Cross-Validation:

### Models Trained

| Model | Description |
|---|---|
| **Logistic Regression** | Baseline linear classifier with scaling |
| **Random Forest** | Ensemble model for feature importance |
| **Gradient Boosting** | Advanced boosting for optimal performance |

### Evaluation Metrics

- **Accuracy** – Overall prediction correctness
- **ROC-AUC** – Trade-off between true & false positive rates
- **F1-Score** – Harmonic mean of precision and recall
- **Classification Report** – Precision, recall, and support per class

---

## Results & Visualizations

The notebook generates several visualizations and outputs:

| File | Content |
|---|---|
| `model_comparison.png` | Performance metrics across all 3 models |
| `feature_importance.png` | Top features driving churn predictions |
| `kfold_boxplot.png` | Cross-validation score distributions |
| `customer_churn_results.png` | Summary of results and insights |

---

## Getting Started

### Prerequisites

- Python 3.7+
- Jupyter Notebook
- Required packages (see below)

### Installation & Execution

1. **Clone/download** the repository
2. **Install dependencies**:
   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn jupyter
   ```

3. **Open Jupyter Notebook**:
   ```bash
   jupyter notebook customer_churn.ipynb
   ```

4. **Run cells sequentially** from top to bottom

### What Happens When You Run It

- Loads the raw dataset
- Cleans and processes the data
- Saves the cleaned data to `churn_cleaned.csv`
- Trains 3 ML models with cross-validation
- Generates visualizations and model reports
- Displays classification metrics and feature importance

---

## Key Dependencies

```
pandas           # Data manipulation
numpy            # Numerical computing
scikit-learn     # Machine learning models & metrics
matplotlib       # Static plotting
seaborn          # Statistical visualizations
jupyter          # Interactive notebooks
xgboost          # Gradient boosting library
joblib           # Model serialization
```

---

## Workflow Overview

### Step 1: Data Cleaning
The notebook loads the raw dataset and performs:
- Duplicate removal
- Missing value imputation (median for numeric columns)
- Text standardization (Gender, Country, City)
- Type conversion for modeling compatibility

### Step 2: Exploratory Data Analysis
Visualizations reveal:
- Churn distribution across customer segments
- Relationship between features and churn
- Customer demographics and behavior patterns
- Lifetime value and engagement trends

### Step 3: Model Training & Evaluation
Using 5-fold Stratified Cross-Validation:
- **Logistic Regression** – Fast baseline with scaling
- **Random Forest** – Feature importance insights
- **Gradient Boosting** – Best predictive performance

Each model is evaluated on Accuracy, ROC-AUC, and F1-Score.

---

## Key Insights

- **Churn Rate**: ~29% of customers churn (1 in 3 customers leave)
- **Cart Abandonment**: Strong correlation with churn behavior
- **Session Duration**: Higher engagement = lower churn risk
- **Age Factor**: Older customers often show higher lifetime value
- **Geographic Patterns**: Churn rates vary by country/region
- **Feature Importance**: Certain features are strong churn predictors

---

## Challenges & Solutions

| Challenge | Solution |
|---|---|
| Missing values in multiple columns | Median imputation for numeric columns |
| Inconsistent text formatting | `.str.strip()` and `.str.capitalize()` for standardization |
| Imbalanced churn distribution | Stratified K-Fold cross-validation |
| Data leakage in preprocessing | Pipeline-based scaling within CV folds |

---

## Files in This Repository

- **`ecommerce_customer_churn_dataset.csv`** – Original raw dataset (50,000 records)
- **`churn_cleaned.csv`** – Cleaned and preprocessed dataset
- **`customer_churn.ipynb`** – Main Jupyter notebook with full analysis
- **`model_comparison.png`** – ML model performance visualization
- **`feature_importance.png`** – Feature importance analysis
- **`kfold_boxplot.png`** – Cross-validation results
- **`customer_churn_results.png`** – Final results summary
- **`README.md`** – This file

---

## Future Enhancements

Possible next steps:
- Feature engineering (create new derived features)
- Hyperparameter tuning for better model performance
- Deploy model as REST API
- Real-time churn prediction pipeline
- Customer retention recommendations based on risk scores

---

## Author & License

Created as part of e-commerce analytics project.
Feel free to fork, modify, and use this project for learning and analysis purposes.

---

**Last Updated**: May 2026

1. Start the container with `docker compose up --build`.
2. Open Jupyter at `http://127.0.0.1:8888`.
3. Run [`notebook/customer_churn.ipynb`](./notebook/customer_churn.ipynb).
4. Review the cleaned dataset in [`data/churn_cleaned.csv`](./data/churn_cleaned.csv).

---

## Future Improvements

- add saved chart outputs to the repository
- add Hadoop mapper and reducer scripts under a dedicated `hadoop/` folder
- build a churn prediction model and compare algorithms
- create a dashboard for interactive exploration
