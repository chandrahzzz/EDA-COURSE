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
| `SLE.ipynb` | **Simple Linear Estimation (SLE)**: Parametric Modeling, OLS Derivation, Diagnostic Hypothesis Testing, and Validation | R (RStudio / Colab) |
| `pca.ipynb` | Dimensionality Reduction: Principal Component Analysis (PCA), Statistical Suitability Diagnostics, and Biplots | R (RStudio / Colab) |
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

## Simple Linear Estimation (`SLE.ipynb`)

An end-to-end econometric and statistical modeling pipeline implemented in R to quantify and evaluate the linear relationship between reading comprehension (`read12`) and mathematics performance (`math12`) from the Catholic High School dataset.

### 1. Exploratory Diagnostics & Bivariate Profiling
- **Data Filtering**: Extraction of complete paired observations for 12th-grade reading (`read12`) and mathematics (`math12`) scores across 7,430 students.
- **Descriptive Statistics**: Evaluation of distributional moments (mean, standard deviation, median, range) across both cognitive metrics.
- **Visual Exploration**: Dual-panel histograms, boxplot dispersions, and bivariate scatter plots inspecting linearity and data density.
- **Correlation Testing**: Pearson product-moment correlation analysis establishing a strong, statistically significant positive linear relationship ($r \approx 0.7104$, $t = 86.998$, $p < 2.2 \times 10^{-16}$, 95% CI $[0.6990, 0.7215]$).

### 2. Ordinary Least Squares (OLS) Formulation & Estimation
- **Train/Test Splitting**: Seeded 80/20 partitioning into a training cohort ($n_{\text{train}} = 5,944$) and holdout test cohort ($n_{\text{test}} = 1,486$).
- **Manual Parameter Derivation**: Explicit calculation of slope and intercept using closed-form analytical formulas:
  $$\hat{\beta}_1 = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2} \approx 0.7147, \quad \hat{\beta}_0 = \bar{y} - \hat{\beta}_1 \bar{x} \approx 15.1242$$
- **Model Fitting (`lm`)**: Estimation via `lm(math12 ~ read12, data = train)`, yielding identical parameter values:
  $$\widehat{\text{math12}} = 15.1242 + 0.7147 \times \text{read12}$$
- **Statistical Significance**: Both parameters are highly significant ($t_{\hat{\beta}_0} = 31.48$, $t_{\hat{\beta}_1} = 78.21$, $p < 2 \times 10^{-16}$).
- **Confidence Intervals**: 95% confidence intervals constructed for the intercept $[14.1824, 16.0659]$ and slope $[0.6967, 0.7326]$.

### 3. Variance Decomposition & Goodness-of-Fit
- **ANOVA Decomposition**: Partitioning total variability into explained and unexplained sums of squares ($F(1, 5942) = 6117.47, p < 2 \times 10^{-16}$):
  - Total Sum of Squares ($SST$): $5.356 \times 10^5$
  - Regression Sum of Squares ($SSR$): $2.717 \times 10^5$
  - Error Sum of Squares ($SSE$): $2.639 \times 10^5$
- **Coefficient of Determination**: In-sample $R^2 \approx 0.5073$ (Adjusted $R^2 \approx 0.5072$), indicating reading aptitude explains approximately $50.7\%$ of the variance in math performance.

### 4. Classical Regression Diagnostics & Assumption Testing
- **Four-Panel Residual Diagnostics**: Residuals vs. Fitted, Normal Q-Q, Scale-Location, and Residuals vs. Leverage plots.
- **Normality Assessment**: Q-Q reference line comparison and Shapiro-Wilk normality test on residuals ($W = 0.9985, p = 9.78 \times 10^{-5}$).
- **Homoscedasticity Assessment**: Studentized Breusch-Pagan test ($BP = 14.801, df = 1, p = 0.00012$) evaluating constancy of residual variance across fitted levels.
- **Independence Assessment**: Durbin-Watson test ($DW = 1.9813, p = 0.236$), establishing absence of first-order autocorrelation in residuals.
- **Influence & Leverage Analysis**: Cook’s Distance computation identifying observations exceeding the leverage threshold ($h = 4/n_{\text{train}}$) to monitor influential outliers.

### 5. Out-of-Sample Generalization & Prediction Bands
- **Holdout Validation**: Evaluating test set generalization against unseen data ($n_{\text{test}} = 1,486$):
  - Root Mean Squared Error (RMSE): $\approx 6.6306$
  - Mean Absolute Error (MAE): $\approx 5.3198$
  - Holdout Test $R^2$: $\approx 0.4937$ ($49.4\%$ variance explained out-of-sample)
- **Actual vs. Predicted Evaluation**: Scatter visualization comparing predicted values against ground truth along the 45-degree reference line ($y = x$).
- **Interval Estimation**: Constructing 95% Narrow Confidence Intervals (mean response estimation) and Wide Prediction Intervals (individual observation forecast) plotted across the entire operational range of reading scores.

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
- lmtest

```R
install.packages(c("dplyr", "ggplot2", "GGally", "reshape2", "e1071", "tidyr", "corrplot", "lmtest"))
```

---

## Sole Contributor & Author

- **Chandrahas Reddy** (Registration No: 23BDS0308)
  - Email: chandrahas2094@gmail.com
  - Sole Author, Researcher, and Maintainer
