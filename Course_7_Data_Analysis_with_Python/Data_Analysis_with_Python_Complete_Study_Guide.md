# IBM Data Science Professional Certificate
## Course 7: Data Analysis with Python — Complete Study Guide & Reference

---

### Course Overview
- **Course Name**: Data Analysis with Python
- **Course Number**: Course 7 of 12
- **Provider**: IBM (Coursera)
- **Status**: **Completed & Passed (95.5% Overall Grade)**
- **Primary Tooling**: Python, Pandas, NumPy, SciPy, Matplotlib, Seaborn, Scikit-learn, Statsmodels

---

### Grade Summary
| Assessment | Weight | Score | Status |
| :--- | :---: | :---: | :---: |
| Graded Quiz: Importing Data Sets | 10% | 100% | Passed |
| Graded Quiz: Data Wrangling | 10% | 100% | Passed |
| Graded Quiz: Exploratory Data Analysis | 10% | 100% | Passed |
| Graded Quiz: Model Development | 10% | 100% | Passed |
| Graded Quiz: Model Evaluation and Refinement | 10% | 80% | Passed |
| AI Graded - Final Project Submission and Evaluation | 25% | 100% | Passed |
| Final Exam | 25% | 90% | Passed |
| **Total Course Grade** | **100%** | **95.5%** | **PASSED** |

---

### Module-by-Module Breakdown & Key Concepts

#### Module 1: Importing Data Sets
1. **Data Acquisition & Format Identification**:
   - Tabular datasets in flat CSV, JSON, Excel, and SQL database formats.
   - Using Pandas pd.read_csv(filepath_or_buffer, headers=None) to ingest datasets into structured DataFrames.
2. **DataFrame Inspection**:
   - df.head(n), df.tail(n) for inspecting leading and trailing rows.
   - df.info(), df.dtypes for data type verification across columns.
   - df.describe(include=\'all\') for statistical summaries of numeric and categorical variables.
3. **Database Integration**:
   - Python DB-API 2.0 interface: standard connection objects, cursor execution (cursor.execute()), and result extraction (etchall()).

---

#### Module 2: Data Wrangling
1. **Handling Missing Values**:
   - Identifying missing values via df.isnull() and df.notnull().
   - Replacing missing numeric values with column central tendencies (df[\'column\'].replace(np.nan, df[\'column\'].mean())).
   - Dropping incomplete rows for essential target labels (df.dropna(subset=[\'price\'], axis=0, inplace=True)).
2. **Data Formatting & Standardization**:
   - Type casting with df[\'column\'] = df[\'column\'].astype(\'float\') or \'int\'.
   - Unit transformations (e.g., converting MPG to L/100km).
3. **Data Normalization Techniques**:
   - Simple feature scaling: x_new = x / x_max
   - Min-Max scaling: x_new = (x - x_min) / (x_max - x_min)
   - Z-score standardization: x_new = (x - mu) / sigma
4. **Binning & One-Hot Encoding**:
   - Equal-width binning using 
p.linspace() and pd.cut() to convert continuous values into categorical groups.
   - Dummy variable transformation with pd.get_dummies() for non-ordinal categorical predictors.

---

#### Module 3: Exploratory Data Analysis (EDA)
1. **Descriptive Statistics**:
   - df.describe(), df[\'column\'].value_counts().to_frame().
2. **Groupby & Pivot Tables**:
   - df.groupby([\'drive-wheels\', \'body-style\'], as_index=False)[\'price\'].mean()
   - Reshaping multi-index tables with df.pivot(index=\'drive-wheels\', columns=\'body-style\') and visualizing with heatmaps (sns.heatmap(), plt.pcolor()).
3. **Correlation Analysis**:
   - **Pearson Correlation Coefficient (r)**: Measures linear association (ranges from -1 to +1; 0 indicates no linear correlation).
   - **P-value**: Quantifies statistical significance (p < 0.001 indicates strong certainty).
   - Using scipy.stats.pearsonr(df[\'engine-size\'], df[\'price\']).
4. **Visual Diagnostics**:
   - Scatter plots with regression trendlines (sns.regplot()).
   - Categorical distribution boxplots (sns.boxplot()).

---

#### Module 4: Model Development
1. **Simple & Multiple Linear Regression**:
   - Formulations: y_hat = b0 + b1*x and y_hat = b0 + b1*x1 + ... + bn*xn.
   - Scikit-learn implementation: rom sklearn.linear_model import LinearRegression.
2. **Polynomial Regression & Pipelines**:
   - Fitting higher-order polynomial curves: PolynomialFeatures(degree=2).
   - Streamlining end-to-end transformation and estimation:
     `python
     from sklearn.pipeline import Pipeline
     from sklearn.preprocessing import StandardScaler, PolynomialFeatures
     from sklearn.linear_model import LinearRegression

     pipe = Pipeline([
         (\'scale\', StandardScaler()),
         (\'polynomial\', PolynomialFeatures(degree=2)),
         (\'model\', LinearRegression())
     ])
     pipe.fit(X, y)
     y_pred = pipe.predict(X)
     `
3. **Model Evaluation Metrics**:
   - **Mean Squared Error (MSE)**: mean_squared_error(y, y_pred).
   - **Coefficient of Determination (R^2)**: r2_score(y, y_pred) or model.score(X, y).
   - **Residual Plots**: Checking for random scatter around zero vs. non-linear / heteroscedastic funnel shapes.

---

#### Module 5: Model Evaluation and Refinement
1. **Train-Test Split & Overfitting Diagnostics**:
   - Partitioning data using 	rain_test_split(X, y, test_size=0.3, random_state=42).
   - Identifying overfitting when Training R^2 >> Test R^2.
2. **Cross-Validation**:
   - cross_val_score(model, X, y, cv=4) for reliable fold scoring.
   - cross_val_predict(model, X, y, cv=4) for unbiased out-of-fold predictions.
3. **Regularization (Ridge & Lasso)**:
   - **Ridge Regression (L2)**: Adds penalty alpha * sum(w_j^2) to shrink coefficients and prevent overfitting.
   - **Lasso Regression (L1)**: Adds penalty alpha * sum(|w_j|) for feature sparsity.
4. **Hyperparameter Tuning with Grid Search**:
   - Automated grid evaluation across hyperparameter ranges:
     `python
     from sklearn.model_selection import GridSearchCV
     from sklearn.linear_model import Ridge

     parameters = [{\'alpha\': [0.001, 0.01, 0.1, 1, 10, 100, 1000]}]
     grid = GridSearchCV(Ridge(), parameters, cv=4)
     grid.fit(X_train, y_train)
     best_model = grid.best_estimator_
     `

---

#### Module 6: Final Project & Exam Key Insights
- **Diagnosing Real Estate Prediction Outputs**:
  - Training R^2 = 0.91, Test R^2 = 0.68 -> Overfitting due to high model complexity or unregularized feature weights.
  - Expanding residual spread -> Heteroscedasticity, solved by target log transformations (
p.log1p(price)).
  - Model refinement pipeline: Feature standardization -> Regularization tuning (alpha) via GridSearchCV -> K-fold cross-validation.
