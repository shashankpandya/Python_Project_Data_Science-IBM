# Course 9: Machine Learning with Python — Complete Study Guide
*IBM Data Science Professional Certificate*

---

## 🎯 Course Overview & Results
- **Overall Final Grade**: **93.57%**
- **Status**: Completed & Passed ✅

### Grade Breakdown:
1. **Module 1**: Graded Quiz: Introduction to Machine Learning — **100%**
2. **Module 2**: Graded Quiz: Linear and Logistic Regression — **85.71%**
3. **Module 3**: Graded Quiz: Building Supervised Learning Models — **85.71%**
4. **Module 4**: Graded Quiz: Building Unsupervised Learning Models — **100%**
5. **Module 5**: Graded Quiz: Evaluating and Validating Machine Learning Models — **85.71%**
6. **Module 6**: Final Exam: Machine Learning with Python — **100%**

---

## 📚 Module-by-Module Summary

### Module 1: Introduction to Machine Learning
- **Core Concept**: Machine learning algorithms discover patterns in historical data to make data-driven decisions or continuous/categorical predictions without explicit rule programming.
- **Model Lifecycle**:
  `Problem Definition` -> `Data Collection` -> `Data Preparation` -> `Model Building & Evaluation` -> `Deployment`
- **Key Ecosystem Libraries**:
  - `numpy`: Multi-dimensional arrays and fast mathematical vectorization.
  - `pandas`: Data structures (DataFrame, Series), data wrangling, cleaning, missing value handling.
  - `scipy`: Scientific computations, statistical distributions, numerical optimization routines.
  - `matplotlib` & `seaborn`: Data visualization, exploratory analysis charts.
  - `scikit-learn`: Unified, production-ready machine learning framework for preprocessing, classical algorithms, evaluation, and pipeline assembly.

---

### Module 2: Linear and Logistic Regression
- **Simple Linear Regression**: Models linear relationship between one independent variable X and dependent continuous variable Y:
  `Y = β0 + β1*X + ε`
- **Multiple Linear Regression**: Extends simple regression to multiple continuous or encoded categorical predictors X1, X2, ..., Xp:
  `Y = β0 + β1*X1 + ... + βp*Xp + ε`
- **Polynomial Regression**: Captures non-linear, smooth curvature patterns by mapping features into higher-degree polynomial feature spaces while fitting with linear coefficients.
- **Logistic Regression (Classification)**:
  - Models probability P(Y=1|X) using the sigmoid/logistic link function:
    `σ(z) = 1 / (1 + e^(-z))`
  - **Decision Threshold Tuning**: Modifying classification threshold (default 0.5) allows balancing Precision vs Recall and reducing False Positives in high-cost domains.
  - **Log-Loss / Cross-Entropy**: Measures distance between predicted probability distributions and true categorical labels; lower values indicate higher calibration and prediction accuracy.

---

### Module 3: Building Supervised Learning Models
- **K-Nearest Neighbors (KNN)**:
  - Non-parametric, distance-based instance learning algorithm.
  - Increasing K provides smoothing and reduces variance, but overly high K values cause underfitting (diluting local structural details).
- **Decision Trees (Classification & Regression)**:
  - Hierarchical tree splitting using optimization criteria:
    - Classification: Information Gain (Entropy Reduction) or Gini Impurity.
    - Regression: Mean Squared Error (MSE) / Variance Reduction; Leaf nodes predict mean (or median for outlier robustness).
  - Deeper trees -> Lower Bias, Higher Variance (risk of overfitting).
- **Support Vector Machines & Regression (SVM / SVR)**:
  - Finds maximum margin hyperplanes separating classes or fitting within an ε-insensitive margin corridor.
  - ε Parameter in SVR defines the error tolerance band where minor deviations incur zero penalty.
- **Ensemble Techniques (AdaBoost & Random Forest)**:
  - **AdaBoost (Adaptive Boosting)**: Sequentially trains weak learners, re-weighting incorrectly predicted samples to convert a collection of weak learners into a single strong, low-bias predictor.
  - **Random Forest**: Bagging ensemble combining decorrelated decision trees to drastically minimize variance and overfitting.

---

### Module 4: Building Unsupervised Learning Models
- **Objective**: Discover hidden natural cluster structures, cluster hierarchies, and manifold geometry in unlabelled datasets.
- **K-Means Clustering**:
  - Partitioning algorithm assigning points to nearest centroid μk.
  - Centroid represents the multi-dimensional arithmetic coordinate mean of all members belonging to that cluster.
  - Optimal K selected using the **Elbow Method** (inertia / WCSS reduction).
- **Hierarchical Clustering**:
  - Agglomerative (bottom-up) or Divisive (top-down) clustering.
  - Visualized via **Dendrograms**, enabling inspection of similarity at multiple granular cut levels without pre-specifying K.
- **DBSCAN (Density-Based Spatial Clustering of Applications with Noise)**:
  - Identifies arbitrarily shaped dense clusters and automatically flags isolated low-density points as noise/outliers (does not force every point into a cluster).
- **Dimensionality Reduction & Manifold Learning**:
  - **PCA (Principal Component Analysis)**: Linear orthogonal projection maximizing explained variance across principal axes.
  - **t-SNE**: Non-linear probabilistic technique preserving local neighborhood structures for 2D/3D visualization.
  - **UMAP**: Non-linear manifold technique preserving both local neighborhood relationships and global geometric structure.

---

### Module 5: Evaluating and Validating Machine Learning Models
- **Classification Performance Metrics**:
  - Accuracy = (TP + TN) / (TP + TN + FP + FN)
  - Precision = TP / (TP + FP) (Crucial when False Positives are costly)
  - Recall / Sensitivity = TP / (TP + FN) (Crucial when False Negatives are costly)
  - F1-Score = 2 * (Precision * Recall) / (Precision + Recall)
- **Regression Evaluation Metrics**:
  - MAE (Mean Absolute Error): Mean magnitude of absolute errors.
  - MSE (Mean Squared Error): Mean squared magnitude of errors.
  - RMSE (Root Mean Squared Error): Square root of MSE (interpretable in original target units).
  - R^2 (Coefficient of Determination): Proportion of target variance explained by predictors.
- **Unsupervised Evaluation**:
  - Silhouette Coefficient: Measures cluster cohesion vs separation ([-1, 1], values near +1 denote distinct, compact clusters).
  - Davies-Bouldin Index: Lower values indicate better clustering separation.
- **Regularization & Generalization**:
  - **Lasso Regression (L1 Penalty)**: Shrinks coefficients strictly to zero, performing automatic feature selection.
  - **Ridge Regression (L2 Penalty)**: Shrinks coefficients toward zero to mitigate multicollinearity.
  - **Data Leakage Mitigation**: Strictly separate training, validation, and test subsets before any scaling, encoding, or feature engineering transforms.
