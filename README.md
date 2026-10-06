# Exploratory Data Analysis (EDA) Coursework

## Student Information

- **Name**: Chandrahas Reddy
- **Registration Number**: 23BDS0308
- **Email**: chandrahas2094@gmail.com

---

## Repository Overview

This repository contains coursework and analytical implementations for Exploratory Data Analysis (EDA), statistical evaluation, and unsupervised clustering applied to educational achievement and socio-economic datasets. 

The primary study utilizes the `catholic.csv` dataset, which records academic performance indicators, family background, demographic attributes, and school sector attendance (Catholic vs. Public high schools).

---

## Repository Structure

| File | Description | Language / Environment |
|---|---|---|
| `Untitled5.ipynb` | **Phase-1**: Comprehensive Exploratory Data Analysis, Data Cleaning, and Visualization | Python (Jupyter / Colab) |
| `1D,2D,NDipynb.ipynb` | One-Dimensional, Two-Dimensional, and Multi-Dimensional Statistical Analysis | R (RStudio / Colab) |
| `HeirarchicalNKmeans.ipynb` | Unsupervised Learning: Hierarchical Clustering and K-Means Partitioning | R (RStudio / Colab) |

---

## Phase-1: Exploratory Data Analysis (`Untitled5.ipynb`)

The primary component of Phase-1 is implemented in `Untitled5.ipynb`. It establishes a complete data analysis pipeline covering data ingestion, cleaning, transformation, and multi-level visual exploration.

### 1. Data Ingestion and Structural Inspection
- Loading the raw dataset containing 7,430 observations across 14 variables.
- Reviewing schema, structural dimensions, column data types, unique counts, and missing entry summaries.

### 2. Missing Value Analysis and Cleaning
- Missing data diagnosis using numerical summaries and visual representation via a missing value heatmap.
- Imputation of missing quantitative measurements using column median values.
- Removal of redundant index tracking columns (`rownames`) and verification of duplicate records.
- Outlier treatment using Interquartile Range (IQR) capping on continuous features to prevent skewing downstream analyses.

### 3. Feature Engineering
- Construction of `avg_score` as the composite mean of 12th-grade reading (`read12`) and mathematics (`math12`) scores.
- Mapping binary indicator columns into descriptive categorical variables (`gender`, `catholic_hs`, and categorized `ethnicity`).

### 4. Univariate Analysis
- Distribution examination of 12th-grade reading scores using histograms and Kernel Density Estimation (KDE).
- Boxplot visualization for math score dispersion and spread.
- Frequency distribution of students across gender and parental education levels (`motheduc`).

### 5. Bivariate Analysis
- Assessment of correlation between reading and math performance using scatter plots.
- Performance comparisons across Catholic and public school cohorts using boxplots on `avg_score`.
- Evaluation of academic outcomes by gender.
- Regression modeling of log family income (`lfaminc`) against academic score performance.

### 6. Multivariate Analysis
- Correlation matrix computation and heatmap visualization across academic scores, parental education, and income metrics.
- Pairwise feature distributions split across school types.
- Interaction analysis assessing student performance conditioned simultaneously on ethnicity and gender.
- Faceted linear regression analysis examining family income versus performance categorized by gender and school type.

### 7. Data Export
- Generation of the preprocessed and transformed dataset exported as `catholic_cleaned.csv`.

---

## Supplemental Coursework Notebooks

### 1D, 2D, and N-Dimensional EDA (`1D,2D,NDipynb.ipynb`)
- Focused R implementation analyzing single-variable statistics (mean, median, variance, skewness, kurtosis).
- Bivariate frequency analysis, contingency tables, and cross-tabulation.
- Higher-dimensional groupings examining socio-economic determinants of academic achievement.

### Clustering Analysis (`HeirarchicalNKmeans.ipynb`)
- Unsupervised learning pipeline in R.
- Standardization of continuous variables (`scale()`).
- Hierarchical clustering using Euclidean distance and agglomerative linkages visualized through dendrograms.
- K-Means clustering with optimal cluster identification and cluster profile characterization.

---

## Requirements and Dependencies

### Python Environment
- Python 3.8+
- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### R Environment
- R 4.0+
- dplyr
- ggplot2
- GGally
- reshape2
- e1071

```R
install.packages(c("dplyr", "ggplot2", "GGally", "reshape2", "e1071"))
```

---

## Contributor

- **Chandrahas Reddy** (23BDS0308) - Repository Author and Maintainer
