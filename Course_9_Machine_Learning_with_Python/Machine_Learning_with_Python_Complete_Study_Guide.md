# Course 9: Machine Learning with Python — Complete Study Guide
**IBM Data Science Professional Certificate — Course 9 of 12**
**Learner**: Pandya Shashank (Pandya Nareshbhai)
**Status**: Completed & Certified — **93.57% Overall Grade**

---

## Course Overview

**Machine Learning with Python** covers the complete lifecycle of classical machine learning — from regression and classification through unsupervised clustering, dimensionality reduction, and model evaluation best practices — using scikit-learn as the primary framework.

**Primary Libraries**: scikit-learn · NumPy · Pandas · Matplotlib · Seaborn

---

## Grade Summary

| Assessment | Weight | Score | Status |
|:---|:---:|:---:|:---:|
| Graded Quiz: Introduction to Machine Learning | 15% | 100% | Passed |
| Graded Quiz: Linear and Logistic Regression | 15% | 85.71% | Passed |
| Graded Quiz: Building Supervised Learning Models | 15% | 85.71% | Passed |
| Graded Quiz: Building Unsupervised Learning Models | 15% | 100% | Passed |
| Graded Quiz: Evaluating and Validating ML Models | 15% | 85.71% | Passed |
| Final Exam: Machine Learning with Python | 25% | 100% | Passed |
| **Overall Course Grade** | **100%** | **93.57%** | **PASSED** |

---

## Module 1: Introduction to Machine Learning

### What is Machine Learning?
Machine learning is a branch of AI where systems learn patterns from historical data to make predictions or decisions without being explicitly programmed for every case.

### ML Task Types
| Task | Output Type | Examples |
|:---|:---|:---|
| **Regression** | Continuous numeric value | House price, blood glucose level, stock price |
| **Classification** | Discrete categorical label | Spam/not-spam, disease/no-disease, digit recognition |
| **Clustering** | Group assignments (unlabeled) | Customer segmentation, anomaly detection |
| **Association** | Co-occurrence rules | Market basket analysis ("customers who buy X also buy Y") |

### Machine Learning Lifecycle
```
1. Problem Definition     → Define KPIs, success criteria, and output type
2. Data Collection        → APIs, databases, web scraping, CSV/Excel
3. Data Preparation       → Cleaning, wrangling, feature engineering
4. Exploratory Analysis   → EDA: distributions, correlations, outliers
5. Model Development      → Algorithm selection, training, hyperparameter tuning
6. Model Evaluation       → Cross-validation, test set metrics, bias-variance diagnostics
7. Deployment             → REST API (Flask/FastAPI), cloud service, model monitoring
```

### Key Python ML Ecosystem
```python
# Core Libraries
import numpy as np                              # Numerical arrays
import pandas as pd                             # DataFrames
import matplotlib.pyplot as plt                 # Plotting
import seaborn as sns                           # Statistical visualization

# Scikit-learn
from sklearn.model_selection import train_test_split, cross_val_score, GridSearchCV
from sklearn.preprocessing import StandardScaler, LabelEncoder, PolynomialFeatures
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LinearRegression, LogisticRegression, Ridge, Lasso
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor
from sklearn.neighbors import KNeighborsClassifier
from sklearn.svm import SVC, SVR
from sklearn.ensemble import RandomForestClassifier, AdaBoostClassifier, GradientBoostingClassifier
from sklearn.cluster import KMeans, DBSCAN, AgglomerativeClustering
from sklearn.decomposition import PCA
from sklearn.metrics import (accuracy_score, precision_score, recall_score, f1_score,
                              confusion_matrix, classification_report, ConfusionMatrixDisplay,
                              mean_squared_error, mean_absolute_error, r2_score,
                              silhouette_score)

# Train-Test Split Template
X_train, X_test, y_train, y_test = train_test_split(X, y,
                                                      test_size=0.2,
                                                      random_state=42,
                                                      stratify=y)   # for classification
```

---

## Module 2: Linear and Logistic Regression

### 2.1 Simple Linear Regression
`Y = β₀ + β₁X + ε`

```python
import numpy as np
import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score
import matplotlib.pyplot as plt

# Load data (e.g., Boston Housing)
from sklearn.datasets import fetch_california_housing
data = fetch_california_housing(as_frame=True)
df = data.frame

X = df[['MedInc']].values   # Single feature
y = df['MedHouseVal'].values

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train
model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

# Coefficients
print(f"Intercept (β₀): {model.intercept_:.4f}")
print(f"Coefficient (β₁): {model.coef_[0]:.4f}")
print(f"R² Score:  {r2_score(y_test, y_pred):.4f}")
print(f"RMSE:      {np.sqrt(mean_squared_error(y_test, y_pred)):.4f}")

# Plot regression line
plt.figure(figsize=(8, 5))
plt.scatter(X_test, y_test, alpha=0.3, color='steelblue', label='Actual')
plt.plot(X_test, y_pred, color='red', linewidth=2, label='Predicted')
plt.xlabel("Median Income")
plt.ylabel("House Value")
plt.title("Simple Linear Regression")
plt.legend(); plt.show()
```

### 2.2 Multiple Linear Regression
```python
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline

# Multiple features
X_multi = df.drop(columns=['MedHouseVal'])
y = df['MedHouseVal']
X_train, X_test, y_train, y_test = train_test_split(X_multi, y, test_size=0.2, random_state=42)

# Pipeline: StandardScaler → LinearRegression
pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('model', LinearRegression())
])
pipe.fit(X_train, y_train)
y_pred = pipe.predict(X_test)

print(f"R² (multiple): {r2_score(y_test, y_pred):.4f}")
```

### 2.3 Polynomial Regression
```python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LinearRegression

# Degree-2 polynomial
poly_pipe = Pipeline([
    ('poly_features', PolynomialFeatures(degree=2, include_bias=False)),
    ('scaler', StandardScaler()),
    ('model', LinearRegression())
])
poly_pipe.fit(X_train, y_train)
print(f"R² (poly deg=2): {r2_score(y_test, poly_pipe.predict(X_test)):.4f}")
```

### 2.4 Logistic Regression
```python
from sklearn.linear_model import LogisticRegression
from sklearn.datasets import load_breast_cancer
from sklearn.metrics import classification_report, ConfusionMatrixDisplay

data = load_breast_cancer(as_frame=True)
X, y = data.data, data.target
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Scale + Logistic Regression
pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('clf', LogisticRegression(max_iter=1000, C=1.0))
])
pipe.fit(X_train, y_train)
y_pred = pipe.predict(X_test)
y_prob = pipe.predict_proba(X_test)[:, 1]

print(classification_report(y_test, y_pred, target_names=data.target_names))

# Confusion Matrix
ConfusionMatrixDisplay.from_predictions(y_test, y_pred, display_labels=data.target_names)
plt.title("Logistic Regression Confusion Matrix"); plt.show()

# Decision Threshold Tuning (reduce false positives)
threshold = 0.3    # Lower threshold → more positives predicted
y_pred_custom = (y_prob >= threshold).astype(int)
```

### 2.5 Ridge & Lasso Regularization
```python
from sklearn.linear_model import Ridge, Lasso
from sklearn.model_selection import GridSearchCV

# Ridge (L2): prevents overfitting, keeps all features
ridge_cv = GridSearchCV(Ridge(), {'alpha': [0.01, 0.1, 1.0, 10, 100]}, cv=5)
ridge_cv.fit(X_train, y_train)
print(f"Best Ridge alpha: {ridge_cv.best_params_['alpha']}")
print(f"Ridge R²: {r2_score(y_test, ridge_cv.predict(X_test)):.4f}")

# Lasso (L1): performs feature selection by zeroing out irrelevant coefficients
lasso_cv = GridSearchCV(Lasso(max_iter=5000), {'alpha': [0.001, 0.01, 0.1, 1, 10]}, cv=5)
lasso_cv.fit(X_train, y_train)
print(f"Best Lasso alpha: {lasso_cv.best_params_['alpha']}")
# Lasso coefficients = 0 indicate dropped features
print("Non-zero Lasso features:", np.sum(lasso_cv.best_estimator_.coef_ != 0))
```

---

## Module 3: Building Supervised Learning Models

### 3.1 K-Nearest Neighbors (KNN)
```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import cross_val_score
import numpy as np

# Find optimal K
k_range = range(1, 31)
cv_scores = [cross_val_score(KNeighborsClassifier(n_neighbors=k), X_train, y_train, cv=5).mean()
             for k in k_range]
best_k = k_range[np.argmax(cv_scores)]
print(f"Best K: {best_k} with CV accuracy: {max(cv_scores):.4f}")

knn = KNeighborsClassifier(n_neighbors=best_k)
knn.fit(X_train, y_train)
print(f"Test Accuracy: {knn.score(X_test, y_test):.4f}")
```

### 3.2 Decision Tree
```python
from sklearn.tree import DecisionTreeClassifier, export_text, plot_tree

dt = DecisionTreeClassifier(
    criterion='entropy',       # 'gini' or 'entropy' (information gain)
    max_depth=5,               # Prevents overfitting
    min_samples_leaf=10,
    random_state=42
)
dt.fit(X_train, y_train)
print(f"Train Accuracy: {dt.score(X_train, y_train):.4f}")
print(f"Test Accuracy:  {dt.score(X_test, y_test):.4f}")

# Visualize the tree
plt.figure(figsize=(20, 8))
plot_tree(dt, max_depth=3, feature_names=X.columns.tolist(),
          class_names=data.target_names, filled=True, rounded=True, fontsize=10)
plt.title("Decision Tree (max_depth=5)"); plt.show()

# Feature importances
importances = pd.Series(dt.feature_importances_, index=X.columns).sort_values(ascending=False)
importances[:10].plot(kind='bar', color='steelblue', figsize=(10, 4))
plt.title("Top 10 Feature Importances"); plt.tight_layout(); plt.show()
```

### 3.3 Support Vector Machine (SVM)
```python
from sklearn.svm import SVC
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline
from sklearn.model_selection import GridSearchCV

# SVM is sensitive to feature scale — always scale first
svm_pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('svm', SVC(kernel='rbf', probability=True))
])

# Hyperparameter tuning (C = regularization, gamma = kernel bandwidth)
param_grid = {'svm__C': [0.1, 1, 10, 100], 'svm__gamma': ['scale', 0.01, 0.001]}
grid_search = GridSearchCV(svm_pipe, param_grid, cv=5, scoring='accuracy', n_jobs=-1)
grid_search.fit(X_train, y_train)

print(f"Best params: {grid_search.best_params_}")
print(f"Best CV accuracy: {grid_search.best_score_:.4f}")
print(f"Test accuracy: {grid_search.score(X_test, y_test):.4f}")
```

### 3.4 Ensemble Methods

#### Random Forest
```python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(
    n_estimators=200,     # Number of trees
    max_depth=10,
    min_samples_leaf=5,
    random_state=42,
    n_jobs=-1             # Use all CPU cores
)
rf.fit(X_train, y_train)
print(f"Random Forest Test Accuracy: {rf.score(X_test, y_test):.4f}")
```

#### AdaBoost
```python
from sklearn.ensemble import AdaBoostClassifier
from sklearn.tree import DecisionTreeClassifier

# AdaBoost sequentially trains weak learners (stumps)
# focusing on misclassified samples
ada = AdaBoostClassifier(
    estimator=DecisionTreeClassifier(max_depth=1),  # Weak learner = stump
    n_estimators=200,
    learning_rate=0.5,
    random_state=42
)
ada.fit(X_train, y_train)
print(f"AdaBoost Test Accuracy: {ada.score(X_test, y_test):.4f}")
```

#### Gradient Boosting
```python
from sklearn.ensemble import GradientBoostingClassifier

gb = GradientBoostingClassifier(n_estimators=200, learning_rate=0.05,
                                  max_depth=4, random_state=42)
gb.fit(X_train, y_train)
print(f"Gradient Boosting Test Accuracy: {gb.score(X_test, y_test):.4f}")
```

### 3.5 One-vs-One & One-vs-All Multi-class Classification
```python
from sklearn.multiclass import OneVsOneClassifier, OneVsRestClassifier
from sklearn.svm import SVC

# OvO: trains C*(C-1)/2 binary classifiers (fewer classifiers but slower for large C)
ovo = OneVsOneClassifier(SVC(kernel='rbf'))
ovo.fit(X_train, y_train)

# OvR: trains C binary classifiers (fewer classifiers needed = faster)
ovr = OneVsRestClassifier(SVC(kernel='rbf', probability=True))
ovr.fit(X_train, y_train)
```

---

## Module 4: Building Unsupervised Learning Models

### 4.1 K-Means Clustering
```python
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
import matplotlib.pyplot as plt

# Always scale features before clustering
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Elbow Method: Find optimal K
inertias = []
k_range = range(2, 11)
for k in k_range:
    km = KMeans(n_clusters=k, random_state=42, n_init=10)
    km.fit(X_scaled)
    inertias.append(km.inertia_)

plt.figure(figsize=(8, 4))
plt.plot(k_range, inertias, 'bo-', markersize=8)
plt.xlabel("Number of Clusters (K)")
plt.ylabel("Inertia (WCSS)")
plt.title("K-Means Elbow Method")
plt.xticks(k_range); plt.grid(True, linestyle='--', alpha=0.5); plt.show()

# Train optimal K
optimal_k = 3
km_final = KMeans(n_clusters=optimal_k, random_state=42, n_init=10)
labels = km_final.fit_predict(X_scaled)
print(f"Cluster assignments: {np.unique(labels, return_counts=True)}")
print(f"Inertia: {km_final.inertia_:.2f}")
```

### 4.2 Hierarchical Clustering
```python
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import dendrogram, linkage
import matplotlib.pyplot as plt

# Dendrogram: visualize cluster merge distances
linked = linkage(X_scaled, method='ward')   # Ward minimizes within-cluster variance

plt.figure(figsize=(12, 6))
dendrogram(linked,
           truncate_mode='lastp', p=20,   # Show only last 20 merges
           leaf_rotation=45, leaf_font_size=10,
           show_leaf_counts=True)
plt.title("Hierarchical Clustering Dendrogram (Ward Linkage)")
plt.xlabel("Sample Index"); plt.ylabel("Distance"); plt.show()

# Apply agglomerative clustering
agg = AgglomerativeClustering(n_clusters=3, linkage='ward')
labels = agg.fit_predict(X_scaled)
```

### 4.3 DBSCAN
```python
from sklearn.cluster import DBSCAN
from sklearn.metrics import silhouette_score

# DBSCAN: Density-Based, no need to specify K
# eps = neighborhood radius, min_samples = minimum core neighbors
dbscan = DBSCAN(eps=0.5, min_samples=10)
labels = dbscan.fit_predict(X_scaled)

n_clusters = len(set(labels)) - (1 if -1 in labels else 0)
n_noise = (labels == -1).sum()
print(f"Clusters found: {n_clusters}")
print(f"Noise points (outliers): {n_noise}")

# Silhouette score (only on non-noise points)
if n_clusters > 1:
    mask = labels != -1
    sil_score = silhouette_score(X_scaled[mask], labels[mask])
    print(f"Silhouette Score: {sil_score:.4f}")
```

### 4.4 PCA — Dimensionality Reduction
```python
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# Reduce to 2 components for visualization
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)

print(f"Explained variance ratio: {pca.explained_variance_ratio_}")
print(f"Total variance retained: {pca.explained_variance_ratio_.sum():.4f}")

# Plot in 2D PCA space
fig, ax = plt.subplots(figsize=(8, 6))
scatter = ax.scatter(X_pca[:, 0], X_pca[:, 1], c=y, cmap='viridis',
                     alpha=0.7, edgecolors='white', s=50)
plt.colorbar(scatter, ax=ax, label='Target Class')
ax.set_title("PCA — 2D Projection")
ax.set_xlabel(f"PC1 ({pca.explained_variance_ratio_[0]*100:.1f}% variance)")
ax.set_ylabel(f"PC2 ({pca.explained_variance_ratio_[1]*100:.1f}% variance)")
plt.show()

# Scree plot: choose number of components
pca_full = PCA()
pca_full.fit(X_scaled)
cumvar = np.cumsum(pca_full.explained_variance_ratio_)
plt.figure(figsize=(8, 4))
plt.plot(range(1, len(cumvar)+1), cumvar, 'bo-')
plt.axhline(y=0.95, color='r', linestyle='--', label='95% threshold')
plt.title("Scree Plot — Cumulative Explained Variance")
plt.xlabel("Number of Components"); plt.ylabel("Cumulative Variance")
plt.legend(); plt.grid(True, alpha=0.5); plt.show()
```

### 4.5 t-SNE Visualization
```python
from sklearn.manifold import TSNE
import matplotlib.pyplot as plt

# t-SNE: non-linear, preserves local neighborhood structure
# Note: t-SNE is stochastic and computationally expensive (not for large N)
tsne = TSNE(n_components=2,
            perplexity=30,      # Controls effective neighborhood size (5–50)
            learning_rate=200,
            n_iter=1000,
            random_state=42)
X_tsne = tsne.fit_transform(X_scaled[:2000])   # Subsample for speed

plt.figure(figsize=(8, 6))
scatter = plt.scatter(X_tsne[:, 0], X_tsne[:, 1], c=y[:2000],
                      cmap='tab10', alpha=0.7, s=20)
plt.colorbar(scatter, label='Class')
plt.title("t-SNE 2D Embedding"); plt.axis('off'); plt.show()
```

---

## Module 5: Evaluating and Validating Machine Learning Models

### 5.1 Classification Metrics
```python
from sklearn.metrics import (accuracy_score, precision_score, recall_score,
                              f1_score, roc_auc_score, roc_curve, ConfusionMatrixDisplay)

y_pred = model.predict(X_test)
y_prob = model.predict_proba(X_test)[:, 1]

print(f"Accuracy:   {accuracy_score(y_test, y_pred):.4f}")
print(f"Precision:  {precision_score(y_test, y_pred):.4f}")   # Minimize FP
print(f"Recall:     {recall_score(y_test, y_pred):.4f}")      # Minimize FN
print(f"F1-Score:   {f1_score(y_test, y_pred):.4f}")
print(f"ROC-AUC:    {roc_auc_score(y_test, y_prob):.4f}")

# ROC Curve
fpr, tpr, thresholds = roc_curve(y_test, y_prob)
plt.figure(figsize=(7, 5))
plt.plot(fpr, tpr, lw=2, color='royalblue', label=f'AUC = {roc_auc_score(y_test, y_prob):.3f}')
plt.plot([0, 1], [0, 1], 'k--', alpha=0.5)
plt.xlabel("False Positive Rate"); plt.ylabel("True Positive Rate")
plt.title("ROC Curve"); plt.legend(); plt.grid(True, alpha=0.5); plt.show()

# Confusion Matrix
ConfusionMatrixDisplay.from_predictions(y_test, y_pred)
plt.title("Confusion Matrix"); plt.show()
```

### 5.2 Regression Metrics
```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
import numpy as np

y_pred = model.predict(X_test)

mae  = mean_absolute_error(y_test, y_pred)
mse  = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2   = r2_score(y_test, y_pred)

print(f"MAE:  {mae:.4f}")
print(f"MSE:  {mse:.4f}")
print(f"RMSE: {rmse:.4f}")
print(f"R²:   {r2:.4f}")

# Residuals plot
residuals = y_test - y_pred
plt.figure(figsize=(8, 4))
plt.scatter(y_pred, residuals, alpha=0.4, color='steelblue', edgecolors='white', s=30)
plt.axhline(y=0, color='red', linestyle='--', linewidth=1.5)
plt.xlabel("Predicted Values"); plt.ylabel("Residuals")
plt.title("Residuals vs Predicted — Checking Heteroscedasticity")
plt.grid(True, linestyle='--', alpha=0.5); plt.show()
```

### 5.3 Cross-Validation
```python
from sklearn.model_selection import cross_val_score, StratifiedKFold

# 5-fold stratified cross-validation (stratified preserves class ratios)
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

cv_scores = cross_val_score(model, X, y, cv=cv, scoring='accuracy')
print(f"CV Scores:  {cv_scores}")
print(f"Mean:       {cv_scores.mean():.4f}")
print(f"Std Dev:    {cv_scores.std():.4f}")
print(f"95% CI:     ({cv_scores.mean()-2*cv_scores.std():.4f}, {cv_scores.mean()+2*cv_scores.std():.4f})")
```

### 5.4 Bias-Variance Trade-off Diagnostic
```python
# Training curve: detects underfitting vs overfitting
from sklearn.model_selection import learning_curve

train_sizes, train_scores, val_scores = learning_curve(
    model, X, y, cv=5,
    train_sizes=np.linspace(0.1, 1.0, 10),
    scoring='accuracy', n_jobs=-1
)

plt.figure(figsize=(9, 5))
plt.plot(train_sizes, train_scores.mean(axis=1), 'o-', color='royalblue', label='Training score')
plt.fill_between(train_sizes,
                 train_scores.mean(1) - train_scores.std(1),
                 train_scores.mean(1) + train_scores.std(1), alpha=0.2, color='royalblue')
plt.plot(train_sizes, val_scores.mean(axis=1), 'o-', color='tomato', label='Validation score')
plt.fill_between(train_sizes,
                 val_scores.mean(1) - val_scores.std(1),
                 val_scores.mean(1) + val_scores.std(1), alpha=0.2, color='tomato')
plt.xlabel("Training Size"); plt.ylabel("Accuracy Score")
plt.title("Learning Curve — Bias-Variance Diagnostic")
plt.legend(); plt.grid(True, alpha=0.5); plt.show()
# High train/low val gap -> Overfitting (high variance)
# Both low -> Underfitting (high bias)
```

### 5.5 Clustering Evaluation
```python
from sklearn.metrics import silhouette_score, davies_bouldin_score

# Silhouette Score: [-1, 1], values near +1 = well-separated compact clusters
sil = silhouette_score(X_scaled, cluster_labels)
print(f"Silhouette Score: {sil:.4f}")

# Davies-Bouldin Index: lower = better clustering separation
db = davies_bouldin_score(X_scaled, cluster_labels)
print(f"Davies-Bouldin Index: {db:.4f}")
```

### 5.6 Data Leakage Prevention
```python
# WRONG — scales entire dataset before splitting (leaks test distribution into training)
scaler = StandardScaler()
X_scaled_wrong = scaler.fit_transform(X)        # ← Leakage!
X_train, X_test = train_test_split(X_scaled_wrong, ...)

# CORRECT — fit scaler ONLY on training data, then transform both splits
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)   # Fit + transform on train only
X_test_scaled  = scaler.transform(X_test)         # Transform only (no re-fit)

# Best practice: use Pipeline so leakage is impossible
pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('model', LogisticRegression())
])
cross_val_score(pipe, X, y, cv=5)   # Pipelines handle fit/transform correctly within each fold
```

---

## Summary Cheat Sheet

### Algorithm Selection Guide
| Problem | Recommended Algorithms |
|:---|:---|
| Predict continuous value | Linear Regression, Ridge, Lasso, Polynomial Regression |
| Binary/multiclass classification | Logistic Regression, SVM, Decision Tree, KNN, Random Forest |
| Small dataset, interpretable | Logistic Regression, Decision Tree |
| Large dataset, high accuracy | Gradient Boosting, Random Forest, SVM (RBF) |
| Unlabeled clustering | K-Means (spherical clusters), DBSCAN (arbitrary shape), Hierarchical |
| Dimensionality reduction | PCA (linear), t-SNE (non-linear, visualization), UMAP (local+global structure) |

### Bias-Variance Summary
| Symptom | Cause | Fix |
|:---|:---|:---|
| High train error + High test error | High Bias (Underfitting) | More features, more complex model |
| Low train error + High test error | High Variance (Overfitting) | Regularization (Ridge/Lasso), more data, pruning, ensemble |
| Both errors similar and low | Good generalization | Deploy! |

### Regularization Quick Reference
| Method | Penalty | Feature Selection | Best For |
|:---|:---|:---|:---|
| **Ridge (L2)** | `α·Σwⱼ²` | No (shrinks all) | Multicollinearity |
| **Lasso (L1)** | `α·Σ|wⱼ|` | Yes (zeroes irrelevant) | Sparse features |
| **ElasticNet** | `α·(L1+L2)` | Yes (partial) | Both problems combined |

---
*Generated and saved: `Course_9_Machine_Learning_with_Python/Machine_Learning_with_Python_Complete_Study_Guide.md`*
*IBM Data Science Professional Certificate — Pandya Shashank*
