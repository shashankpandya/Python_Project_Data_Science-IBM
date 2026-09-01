# Course 3: Data Science Methodology - Complete Study Guide
IBM Data Science Professional Certificate - Course 3 of 12
Learner: Pandya Shashank | Status: Completed - 100%

---

## Course Overview

Data Science Methodology introduces the structured, disciplined framework used by professional data scientists to solve real-world problems. The IBM Foundational Methodology (based on John Rollins' CRISP-DM variant) provides a 10-stage iterative framework from business understanding through deployment and feedback.

---

## The IBM Data Science Methodology - All 10 Stages

### Stage 1: Business Understanding
Goal: Translate a vague business problem into a precise, answerable data science question.
- What is the core business question?
- Who are the stakeholders and what decisions will this support?
- What does success look like? Define KPIs and quantifiable success criteria.
- What type of question is it: Descriptive (what happened?), Predictive (what will happen?), or Prescriptive (what should we do?)?

Key Deliverable: A clear, specific, and actionable problem statement.

Example: "Can we predict which hospital patients are at high risk of 30-day readmission so that care coordinators can proactively intervene?"

### Stage 2: Analytic Approach
Goal: Select the appropriate type of analytical model given the question type.

| Business Question | Required Approach | Algorithms |
|:---|:---|:---|
| Is this A or B? (Classification) | Supervised - Classification | Decision Tree, Logistic Regression, SVM, Random Forest |
| How many / how much? (Prediction) | Supervised - Regression | Linear Regression, Ridge, Lasso, Polynomial |
| What are the natural groups? (Discovery) | Unsupervised - Clustering | K-Means, DBSCAN, Hierarchical |
| What items tend to appear together? (Rules) | Unsupervised - Association | Apriori, FP-Growth |
| Is this normal or anomalous? (Detection) | Anomaly Detection | Isolation Forest, One-Class SVM, Autoencoders |

Key Decision - Supervised vs. Unsupervised:
- Supervised: Labeled historical data exists -> train a model to predict the label for new data
- Unsupervised: No labels available -> find hidden patterns or natural groupings in data

### Stage 3: Data Requirements
Goal: Define what data is needed to answer the question.
- What features and fields are needed?
- What is the minimum sample size for statistical power?
- What time period must the data cover?
- What is the required granularity? (row = patient? transaction? day?)
- What data quality requirements must be met?

### Stage 4: Data Collection
Goal: Gather data from all relevant sources.
- Primary Sources: APIs (RESTful JSON/XML), web scraping, surveys, IoT sensors
- Secondary Sources: SQL databases, CSV/Excel files, Kaggle, Data.gov, IBM Watson Studio
- Key Check: Is the data representative? Are there collection biases?
- Document all sources, collection dates, and any known limitations

### Stage 5: Data Understanding (Exploratory Data Analysis)
Goal: Explore data to assess quality, completeness, and feature relationships.

Key EDA actions in Python:
  df.dtypes                          # Verify column types
  df.describe(include='all')          # Statistical summary for all columns
  df.isnull().sum()                   # Missing value count per column
  df.duplicated().sum()               # Duplicate row count
  df['target'].value_counts()         # Class distribution (classification)
  df.corr()                           # Pearson correlation matrix
  sns.heatmap(df.corr(), annot=True)  # Correlation heatmap

Visualizations for EDA:
  - Histograms / KDE plots -> univariate distributions
  - Box plots -> spread, median, and outliers per category
  - Scatter plots -> bivariate relationships
  - Correlation heatmaps -> multivariate linear relationships
  - Pair plots -> all pairwise combinations

Key question after EDA: "Is there a signal in this data that can answer the business question?"

### Stage 6: Data Preparation
Goal: Transform raw data into a clean, ML-ready feature matrix X and target vector y.

This stage typically consumes 60-80% of the total project time in practice!

| Task | Technique | Python Code |
|:---|:---|:---|
| Handle missing values | Mean/median imputation | df['col'].fillna(df['col'].mean()) |
| Remove duplicates | Drop identical rows | df.drop_duplicates(inplace=True) |
| Fix data types | Cast to correct type | df['col'] = df['col'].astype('float') |
| Normalize numeric | Min-Max or Z-score | StandardScaler(), MinMaxScaler() |
| Encode categorical | One-hot encoding | pd.get_dummies(df, drop_first=True) |
| Feature engineering | Derive new features | df['BMI'] = df['weight'] / df['height']**2 |
| Handle outliers | IQR filter / log transform | df[df['col'] < df['col'].quantile(0.99)] |
| Binning | Equal-width bins | pd.cut(df['age'], bins=5) |

### Stage 7: Modeling
Goal: Build, train, and compare multiple candidate models.

Standard ML workflow:
  1. Split data: X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
  2. Select baseline model (simplest reasonable model first)
  3. Train: model.fit(X_train, y_train)
  4. Predict: y_pred = model.predict(X_test)
  5. Evaluate with appropriate metric
  6. Try multiple algorithms and compare
  7. Tune hyperparameters: GridSearchCV or RandomizedSearchCV

Pipeline pattern (prevents data leakage):
  from sklearn.pipeline import Pipeline
  from sklearn.preprocessing import StandardScaler
  from sklearn.linear_model import LogisticRegression
  pipe = Pipeline([('scaler', StandardScaler()), ('model', LogisticRegression())])
  pipe.fit(X_train, y_train)

### Stage 8: Evaluation
Goal: Determine whether the model meets the success criteria from Stage 1.

Classification Metrics:
| Metric | Formula | Best For |
|:---|:---|:---|
| Accuracy | (TP+TN)/(TP+TN+FP+FN) | Balanced classes |
| Precision | TP/(TP+FP) | When FP is costly (spam filter) |
| Recall (Sensitivity) | TP/(TP+FN) | When FN is costly (disease diagnosis) |
| F1-Score | 2*(Precision*Recall)/(Precision+Recall) | Imbalanced classes |
| ROC-AUC | Area under ROC curve | Overall classifier discrimination |

Regression Metrics:
- MAE (Mean Absolute Error): Average absolute difference, same units as target
- MSE (Mean Squared Error): Penalizes large errors heavily
- RMSE: Square root of MSE, same units as target
- R-squared: Proportion of variance explained (0 to 1, higher is better)

Confusion Matrix Terminology:
- TP (True Positive): Predicted POSITIVE, Actual POSITIVE -- Correct!
- TN (True Negative): Predicted NEGATIVE, Actual NEGATIVE -- Correct!
- FP (False Positive / Type I Error): Predicted POSITIVE, Actual NEGATIVE -- False alarm
- FN (False Negative / Type II Error): Predicted NEGATIVE, Actual POSITIVE -- Missed case

### Stage 9: Deployment
Goal: Make the model accessible to end users and integrate into business processes.

Deployment options (by complexity):
- REST API (Flask, FastAPI) -> accessible to web apps and mobile
- IBM Watson Machine Learning -> scored directly from Watson Studio
- Batch scoring pipeline -> scheduled overnight bulk predictions
- Interactive dashboard (Dash, Streamlit) -> for business analysts
- Edge deployment (ONNX, TensorFlow Lite) -> on-device inference (IoT, mobile)

### Stage 10: Feedback
Goal: Collect real-world performance data and iterate to improve the model.
- Monitor prediction accuracy as new ground-truth labels accumulate
- Detect model drift (distribution shift in real input data over time)
- Re-train model periodically on fresh data
- Refine feature engineering based on production insights
- Escalate back to Stage 1 if the business question has evolved

---

## CRISP-DM vs IBM Methodology Comparison

| CRISP-DM Phase | IBM Methodology Stages |
|:---|:---|
| 1. Business Understanding | Stage 1: Business Understanding |
| 2. Data Understanding | Stage 2-3: Analytic Approach + Data Requirements |
| 3. Data Preparation | Stage 4-6: Collection + Understanding + Preparation |
| 4. Modeling | Stage 7: Modeling |
| 5. Evaluation | Stage 8: Evaluation |
| 6. Deployment | Stage 9-10: Deployment + Feedback |

Both are ITERATIVE - any stage can loop back to a previous stage as insights emerge.

---

## Data Science Methodology Flow

  Business Problem
    -> Analytic Approach (which model type?)
      -> Data Requirements (what data do I need?)
        -> Data Collection (gather the data)
          -> Data Understanding (EDA - is there signal?)
            -> Data Preparation (clean, transform, engineer)
              -> Modeling (train multiple models)
                -> Evaluation (does it meet the criteria?)
                  -> Deployment (serve predictions to users)
                    -> Feedback (monitor, detect drift, retrain)

Every arrow can also point backward - iteration is expected and healthy.

---

## Assessment and Grade Summary

| Assessment | Score |
|:---|:---:|
| Graded Quiz: From Problem to Approach | 100% |
| Graded Quiz: From Requirements to Collection | 100% |
| Graded Quiz: From Understanding to Preparation | 100% |
| Graded Quiz: From Modeling to Evaluation | 100% |
| Final Exam | 100% |
| **Overall Grade** | **100%** |

---

## Master Cheat Sheet

| Concept | Key Point |
|:---|:---|
| Business Understanding | Define the precise question BEFORE touching data |
| Analytic Approach | Match question type to model type |
| Supervised vs Unsupervised | Labeled data -> supervised; no labels -> unsupervised |
| Data Preparation | Takes 60-80% of total project time |
| Confusion Matrix | TP/TN/FP/FN framework for classifier evaluation |
| Precision | Minimize false positives (FP) |
| Recall | Minimize false negatives (FN) |
| F1-Score | Harmonic mean of Precision and Recall |
| Model Drift | Real-world distribution shifts -> re-training required |
| CRISP-DM | Industry-standard 6-phase iterative DS methodology |

---
IBM Data Science Professional Certificate - Pandya Shashank
