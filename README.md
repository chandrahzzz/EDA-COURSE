# EDA Course 
mail : chandrahas2094@gmail.com

Name: Chandrahas Reddy
Reg. No: 23BDS0308

This repo has my work for the Exploratory Data Analysis course. All the notebooks use the same dataset, `catholic.csv`, and go through EDA, linear regression, PCA and clustering. This README only covers the Python notebooks. The R versions of some of these are also in the repo but I have not described them here.

## Dataset

I used the Catholic High School dataset, loaded straight from this link so nothing needs to be downloaded:

```
https://raw.githubusercontent.com/salemprakash/EDA/main/Data/catholic.csv
```

It has 7430 rows and 14 columns. Each row is one student.

| Column | What it is |
|---|---|
| rownames, id | Row number and student id (dropped in most notebooks) |
| read12 | 12th grade reading score |
| math12 | 12th grade math score |
| female | 1 = female, 0 = male |
| asian, hispan, black | Ethnicity indicators (all 0 = white/other) |
| motheduc, fatheduc | Mother's and father's years of education |
| lfaminc | Log of family income |
| hsgrad | Graduated high school (1460 missing values) |
| cathhs | Went to a Catholic high school |
| parcath | Parent is Catholic |

Only 452 of the 7430 students (about 6%) went to a Catholic high school, so the two groups are very unbalanced.

## Notebooks

| Notebook | Topic |
|---|---|
| `Untitled5.ipynb` | Phase 1 - full EDA, cleaning and visualisation |
| `1D,2D,ND.ipynb` | One-dimensional, two-dimensional and N-dimensional analysis |
| `SLE23bds0308.ipynb` | Simple linear regression (math12 on read12) |
| `pca23bds0308.ipynb` | Principal Component Analysis |
| `23bds0308.ipynb` | Hierarchical and K-Means clustering |

---

## 1. Phase 1 EDA (`Untitled5.ipynb`)

This was the first notebook where I went through the whole dataset from start to end.

**Looking at the data**
- Checked shape, data types, `describe()`, number of unique values and missing values for every column.
- Made a missing values table (count and %) and a heatmap of nulls. Only `hsgrad` has missing values.

**Cleaning**
- Filled missing numeric values with the median and categorical ones with the mode.
- Dropped the `rownames` column and checked for duplicate rows.
- Capped outliers in `read12`, `math12`, `motheduc`, `fatheduc` and `lfaminc` using the IQR rule (1.5 x IQR).

**New columns**
- `avg_score` = mean of read12 and math12.
- Turned the 0/1 columns into readable labels: `gender`, `catholic_hs`, `hs_grad`, and one `ethnicity` column made from asian/hispan/black.

**Univariate plots**
- Histogram with KDE for reading scores
- Boxplot for math scores
- Count of students by gender
- Distribution of mother's education

**Bivariate plots**
- Reading vs math scatter plot
- Average score for Catholic vs non-Catholic schools (boxplot)
- Average score by gender
- Log family income vs average score with a regression line

**Multivariate plots**
- Correlation heatmap of the main numeric columns
- Pairplot coloured by Catholic school
- Average score by ethnicity and gender
- `lmplot` of income vs score split by gender and school type

At the end the cleaned data is saved as `catholic_cleaned.csv`.

---

## 2. 1D, 2D and ND Analysis (`1D,2D,ND.ipynb`)

Here I looked at the data one variable at a time, then two at a time, then more than two.

**1D**
- Summary stats for `read12`: mean 51.77, median 53.08, SD 9.41, range 29.15 to 68.09, skewness -0.37, kurtosis -0.83. So reading scores are slightly left skewed and a bit flatter than normal.
- Frequency table, pie chart and bar plot for `cathhs` (6978 No, 452 Yes).
- Binned frequency table (bins of 5), histogram and boxplot for `read12`.

**2D**
- Contingency table of gender vs Catholic HS, with row and column percentages. 6.83% of males and 5.39% of females went to a Catholic school.
- Stacked and grouped bar plots for the same table.
- Box plot and violin plot of math score by Catholic HS.
- Pearson (0.710) and Spearman (0.710) correlation between reading and math, with a scatter plot and regression line.

**ND**
- Mean and SD of math score grouped by gender and Catholic HS. Catholic school students scored higher in both genders (males 56.31 vs 52.68, females 54.29 vs 51.23).
- Scatter of read12 vs math12 coloured by school type. The correlation is 0.71 for non-Catholic and 0.62 for Catholic students.
- Correlation matrix and heatmap of read12, math12, motheduc, fatheduc and lfaminc.
- Pairplot of the five numeric columns.

---

## 3. Simple Linear Regression (`SLE23bds0308.ipynb`)

The goal here was to predict `math12` from `read12` using a simple linear model.

**Before modelling**
- Descriptive stats, histograms and boxplots of both scores.
- Scatter plot of math vs read, which looks clearly linear.
- Pearson correlation test: r = 0.7104, t = 86.998, p close to 0, 95% CI [0.699, 0.7215].

**Fitting the model**
- 80/20 train test split (`random_state=123`) giving 5944 train rows and 1486 test rows.
- Calculated the slope and intercept by hand with the least squares formulas first, then fitted the same model with `statsmodels` OLS to check. Both gave the same answer.

Final equation:

```
math12 = 15.1380 + 0.7142 * read12
```

- 95% CI for the intercept: [14.198, 16.078]
- 95% CI for the slope: [0.696, 0.732]
- ANOVA table: F = 6127.95, p close to 0
- SST = 529962.5, SSR = 269063.5, SSE = 260898.9
- R2 = 0.5077, so reading score explains about half of the variation in math score.

**Checking assumptions**
- Four diagnostic plots: residuals vs fitted, Q-Q plot, scale-location and residuals vs leverage.
- Residual histogram and Shapiro-Wilk test (W = 0.9991, p = 0.014). Residuals are very close to normal, but with this many rows even a tiny difference shows up as significant.
- Breusch-Pagan test (BP = 21.54, p < 0.001), so there is some heteroscedasticity.
- Durbin-Watson = 2.06, so no autocorrelation in the residuals.
- Cook's distance plot with a 4/n cutoff. 188 points are above it.

**Testing on unseen data**
- RMSE = 6.78
- MAE = 5.49
- Test R2 = 0.492, which is close to the training R2, so the model is not overfitting.
- Actual vs predicted plot with the y = x line.
- Confidence and prediction intervals for reading scores of 40, 50 and 60. For example at read12 = 50 the predicted math score is 50.85, with a prediction interval of 37.86 to 63.84.

---

## 4. Principal Component Analysis (`pca23bds0308.ipynb`)

I ran PCA on the five numeric columns: `read12`, `math12`, `motheduc`, `fatheduc` and `lfaminc`. They are on very different scales (scores out of ~70, education in years, income in logs) so I standardized them first.

**Is the data suitable for PCA?**
- Correlation matrix shows the variables are positively correlated, most strongly read12 and math12 (0.71) and mother's and father's education (0.58).
- Bartlett's test of sphericity: chi-square = 11809.68, df = 10, p close to 0. So the correlation matrix is not an identity matrix.
- KMO = 0.704 overall, with every variable's MSA above 0.6. This is good enough for PCA.

**Results**

| PC | Eigenvalue | Variance % | Cumulative % |
|---|---|---|---|
| PC1 | 2.622 | 52.44 | 52.44 |
| PC2 | 1.054 | 21.08 | 73.52 |
| PC3 | 0.626 | 12.51 | 86.03 |
| PC4 | 0.413 | 8.26 | 94.29 |
| PC5 | 0.286 | 5.71 | 100.00 |

- The Kaiser rule (eigenvalue > 1) says keep 2 components, and the 80% rule says 3. I kept 2, which explain about 73.5% of the variance. The scree plot shows the same thing.

**What the components mean**
- PC1 has positive loadings on all five variables (0.40 to 0.47). It is basically an overall "academic and family background" score. Students high on PC1 have good scores and better educated, higher income parents.
- PC2 is positive for read12 and math12 and negative for the parent education and income variables. It separates students whose test scores are higher than their background would suggest from those whose scores are lower.
- I also made contribution bar charts for PC1 and PC2 (with the 20% reference line) and a correlation circle.

**Plots and group comparison**
- Biplot of PC1 vs PC2 coloured by school type, with the variable arrows.
- PC score plots split by race, gender, parent's religion and graduation status.
- Boxplots of PC1 by school, gender and race.
- Catholic school students have a mean PC1 of 0.78 compared to -0.05 for the rest. A Welch t-test gives t = -12.01, p < 0.001, so the difference is significant.

---

## 5. Hierarchical and K-Means Clustering (`23bds0308.ipynb`)

Same five numeric columns as the PCA, standardized with `StandardScaler`.

**Hierarchical clustering**
- Took a random sample of 300 students (seed 123) so the dendrogram is readable.
- Used Ward's linkage with Euclidean distance and plotted the dendrogram.
- Cut the tree into 3 clusters, with sizes 143, 49 and 108.

**K-Means**
- First tried k = 2 (`n_init=25`). Cluster sizes 3594 and 3836, between SS / total SS = 34.2%. It just splits the students into a "below average on everything" group and an "above average on everything" group.
- Used the elbow method (WCSS for k = 1 to 10) to pick k. The bend is around k = 3.
- Final model with k = 3. Cluster sizes 2493, 2036 and 2901, between SS / total SS = 44.3%.

Cluster averages in the original units:

| Cluster | read12 | math12 | motheduc | fatheduc | lfaminc |
|---|---|---|---|---|---|
| 1 | 41.99 | 42.57 | 12.27 | 12.31 | 9.90 |
| 2 | 57.83 | 59.35 | 15.34 | 16.16 | 10.93 |
| 3 | 55.93 | 55.29 | 12.90 | 13.11 | 10.34 |

- Cluster 1: low scores and lower parent education and income.
- Cluster 2: high scores and the most educated, highest income parents.
- Cluster 3: fairly high scores but average family background. This is the same pattern PC2 picked up in the PCA.

---

## How to run

Every notebook has an "Open in Colab" badge at the top (except `Untitled5.ipynb`) and reads the data from the URL above, so the easiest way is to open them in Google Colab and run all cells.

To run locally:

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn statsmodels jupyter
jupyter notebook
```

Libraries used:
- pandas, numpy
- matplotlib, seaborn
- scipy (stats tests, hierarchical clustering)
- scikit-learn (StandardScaler, PCA, KMeans, train_test_split)
- statsmodels (OLS, ANOVA, Breusch-Pagan, Durbin-Watson)

## Author

Chandrahas Reddy (23BDS0308)
chandrahas2094@gmail.com
