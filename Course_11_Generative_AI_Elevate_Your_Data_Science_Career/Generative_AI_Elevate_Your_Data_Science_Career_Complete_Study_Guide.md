# Course 11: Generative AI — Elevate Your Data Science Career
## Complete Comprehensive Study Guide & Exam Reference

---

## 📌 Course Overview
Generative AI is transforming the Data Science landscape by empowering data professionals to automate routine tasks, generate high-fidelity synthetic data, accelerate exploratory data analysis (EDA), engineer novel features, query databases using natural language, and build predictive model pipelines with unprecedented speed.

---

## 🧠 Module 1: Data Science and Generative AI

### 1. Fundamentals of Generative AI in Data Science
* **Traditional vs. Generative AI**:
  * *Traditional / Discriminative AI*: Learns the boundary between classes P(Y|X) (e.g., classification, regression).
  * *Generative AI*: Learns the underlying data distribution P(X) or joint probability P(X, Y) to generate novel, realistic data instances that mimic real-world datasets.
* **Core Modalities & Applications**:
  * **Text Generation**: LLMs (GPT-4, Claude, LLaMA, Granite) generate code snippets, SQL queries, documentation, and executive summaries.
  * **Image Generation & Augmentation**: GANs, Diffusion Models, and VAEs create synthetic medical images, fashion apparel prototypes, and autonomous driving scenarios.
  * **Audio & Sequential Data**: Autoregressive models compose melodies, synthesize speech, and generate financial time series.
  * **Molecular Design & Drug Discovery**: Generative models simulate molecular structures and their biological binding affinities to discover new medications.

---

### 2. Four Core Types of Generative AI Models

| Model Architecture | Mechanism | Key Strengths | Primary Data Science Use Case |
| :--- | :--- | :--- | :--- |
| **Generative Adversarial Networks (GANs)** | Two neural networks pitted against each other: **Generator** (creates candidates) vs. **Discriminator** (evaluates authenticity). | Generates remarkably realistic, high-fidelity synthetic data. | Image data augmentation (CycleGAN, StyleGAN2), balancing imbalanced datasets (CTGAN). |
| **Variational Autoencoders (VAEs)** | **Encoder** maps input data to a probabilistic latent space; **Decoder** reconstructs samples from latent code. | Superb data compression without losing information; smooth latent representation. | Anomaly detection, dimensionality reduction, continuous feature generation. |
| **Autoregressive Models** | Predicts the next element in a sequence conditioned on all previous tokens: P(x_t | x_<t). | **Sequential data champions**; handles long-range temporal dependencies. | Natural language processing (code/text generation), time-series forecasting. |
| **Flow-based Generative Models** | Uses invertible transformations with tractable Jacobians to directly model probability distributions. | **Direct probability modeling** & exact likelihood estimation; efficient exact sampling. | High-precision scientific simulations, density estimation. |

---

### 3. Generative AI in the Data Science Lifecycle

* **Problem Definition**: Generates hypotheses, benchmarks competitive architectures, explores analog solutions in other industries.
* **Data Collection & Generation**: Balances heavily skewed datasets (e.g., fraud detection where positive class < 0.1%) using tools like Synthetic Data Vault (SDV) and CTGAN.
* **Tackling Missing Values**: Generative models learn multivariate correlations to impute plausible, distribution-aligned values rather than simple mean/median replacements.
* **Detecting Outliers**: Learns the exact probability boundaries of standard data distributions; points falling in low-density density regions are flagged.
* **Data Querying**: Translates natural language questions into executable SQL queries (Text-to-SQL).

---

## 🛠️ Module 2: Use of Generative AI for Data Science

### 1. Data Understanding & Exploratory Data Analysis (EDA)
* **Hypothesis Generation**: Generative AI tools automatically scan correlations and distribution shifts to propose domain-specific hypotheses for investigation.
* **Automated Visual Verification**: Generates box plots, histograms, and pair plots to inspect distribution skews and multi-collinearity.

### 2. Predictive Model Building & AutoML Tools
* **Dedicated Generative / AutoML Tools**: DataRobot, H2O Driverless AI, Google Cloud Vertex AI, AutoGluon.
* **Open-Source & Conversational Generative Tools**: ChatGPT, Claude, GitHub Copilot, Gemini.

### 3. Key Considerations & Challenges for Data Professionals
* **Technical Considerations**: Model robustness against adversarial attacks, explainability via **Feature Attribution** (SHAP, LIME).
* **Ethical Considerations**: Informed consent, transparency, bias mitigation, and strict data privacy compliance (GDPR, HIPAA).
* **Organizational Challenges**: Change management, calculating ROI, and mitigating **AI hallucinations**.

---

## 🎯 Quick Exam Review & Summary Points
1. **GAN Purpose**: Two neural networks (Generator + Discriminator) that generate realistic synthetic data.
2. **Sequential Champion**: Autoregressive models (e.g., Transformers, RNNs).
3. **Data Compression**: Variational Autoencoders (VAEs) compress data via low-dimensional latent space.
4. **Direct Density Modeling**: Flow-based models enable exact likelihood sampling.
5. **Text-to-SQL**: Translates natural language prompts into executable SQL queries (e.g., `UPDATE Boston_house_price SET ZN = NULL WHERE ZN = 0`).
6. **Outlier Detection**: Isolation Forest and VAE reconstruction loss identify anomalies beyond standard distribution boundaries.
7. **Simulation & Reinforcement Learning**: Unity ML-Agents provides realistic simulation environments.
8. **AutoML Open-Source**: AutoGluon simplifies training multi-modal and tabular predictive pipelines.
