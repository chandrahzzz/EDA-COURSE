# Exploratory Data Analysis (EDA) Coursework

## Student Information

- **Name**: Chandrahas Reddy
- **Registration Number**: 23BDS0308
- **Email**: chandrahas2094@gmail.com

---

## Repository Overview

This repository contains coursework and analytical implementations for Exploratory Data Analysis (EDA), statistical evaluation, unsupervised clustering, and dimensionality reduction (Principal Component Analysis) applied to educational achievement and socio-economic datasets. 

The primary study utilizes the `catholic.csv` dataset, which records academic performance indicators, family background, demographic attributes, and school sector attendance (Catholic vs. Public high schools).

---

## Repository Structure

| File | Description | Language / Environment |
|---|---|---|
| `Untitled5.ipynb` | **Phase-1**: Comprehensive Exploratory Data Analysis, Data Cleaning, and Visualization | Python (Jupyter / Colab) |
| `1D,2D,NDipynb.ipynb` | One-Dimensional, Two-Dimensional, and Multi-Dimensional Statistical Analysis | R (RStudio / Colab) |
| `HeirarchicalNKmeans.ipynb` | Unsupervised Learning: Hierarchical Clustering and K-Means Partitioning | R (RStudio / Colab) |
| `pca.ipynb` | Dimensionality Reduction: Principal Component Analysis (PCA), Statistical Suitability Diagnostics, and Biplots | R (RStudio / Colab) |

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

## Principal Component Analysis (`pca.ipynb`)

An end-to-end Dimensionality Reduction pipeline implemented in R to analyze continuous performance metrics and socio-economic determinants from the Catholic High School dataset.

### 1. Feature Selection and Preprocessing
- Extraction of continuous variables for PCA: 12th-grade reading score (`read12`), mathematics score (`math12`), maternal education (`motheduc`), paternal education (`fatheduc`), and log family income (`lfaminc`).
- Filtering for complete cases and defining categorical factor variables (`gender`, `school`, `parent`, `race`, `hsgrad_f`) for visual cohort stratification.
- Scale, range, and distribution examination across selected continuous variables.

### 2. Statistical Suitability Diagnostics
- **Correlation Matrix**: Computation and visualization of inter-feature correlations using `corrplot`.
- **Bartlett's Test of Sphericity**: Formal hypothesis test ($H_0$: correlation matrix is an identity matrix, $p < 0.0001$), establishing significant inter-variable correlation appropriate for dimension reduction.
- **Kaiser-Meyer-Olkin (KMO) Measure**: Evaluation of sampling adequacy yielding an overall KMO of ~0.70 with individual Measure of Sampling Adequacy (MSA) values, confirming suitability for factor extraction.

### 3. Eigenvalue Decomposition and Dimensionality Reduction
- Standardization and PCA computation via `prcomp(..., center = TRUE, scale. = TRUE)`.
- **Eigenvalue Decomposition**:
  - **PC1**: Eigenvalue $\approx 2.58$, explaining $\approx 51.6\%$ of total variance.
  - **PC2**: Eigenvalue $\approx 1.07$, explaining $\approx 21.4\%$ of total variance.
  - **Cumulative Explained Variance**: The first two principal components capture $\approx 73-74\%$ of the total data variance.
- **Retention Criteria**: Component retention supported by Kaiser's Rule (eigenvalues $> 1$) and the cumulative variance threshold, selecting $k = 2$ components.

### 4. Component Interpretations and Loadings
- **Component Loadings & Contributions**: Inspection of rotation vectors and percentage contributions relative to the baseline threshold ($100/p = 20\%$).
  - **PC1 ("Academic & Socio-Economic Advantage")**: Uniform directional loadings across test scores and socio-economic features representing overall student background and academic aptitude.
  - **PC2 ("Academic Performance vs. Socio-Economic Background")**: Contrasting loadings separating cognitive achievement (`read12`, `math12`) from family background (`motheduc`, `fatheduc`, `lfaminc`).
- **Correlation Circle**: Graphical projection of feature vectors onto the unit circle detailing variable correlations and projections onto PC1 and PC2.

### 5. Score Projections and Demographic Stratification
- **PCA Biplot**: 2D projection of student observations onto PC1 and PC2 with variable vectors and 95% concentration ellipses stratified by school sector (`Catholic HS` vs. `Non-Catholic HS`).
- **Demographic Projections**: PC score mappings stratified across student race, gender, parental religion, and graduation status.
- **Group Comparison**: Boxplot distributions and an independent two-sample t-test (`t.test(PC1 ~ school)`) comparing mean PC1 scores between Catholic and non-Catholic cohorts.

### 6. Reconstruction Quality and Error Metrics
- Projection matrix reconstruction: $\hat{Z} = \text{Scores}_{1:k} \times \text{Loadings}_{1:k}^T$.
- Measurement of standardized reconstruction error through Mean Squared Reconstruction Error (MSRE).
- Individual feature recovery correlation demonstrating high fidelity for standardized test scores ($r \approx 0.93$) and moderate fidelity for family income ($r \approx 0.75$).

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
- tidyr
- corrplot

```R
install.packages(c("dplyr", "ggplot2", "GGally", "reshape2", "e1071", "tidyr", "corrplot"))
```

---

## Contributor

- **Chandrahas Reddy** (23BDS0308) - Repository Author and Maintainer
