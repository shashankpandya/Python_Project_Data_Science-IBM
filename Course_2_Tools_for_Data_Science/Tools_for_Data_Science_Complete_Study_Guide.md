# Course 2: Tools for Data Science - Complete Study Guide
IBM Data Science Professional Certificate - Course 2 of 12
Learner: Pandya Shashank | Status: Completed - 100%

---

## Module 1: Languages, Libraries and Tools

### Data Science Language Comparison
| Language | Strengths | Primary Use Case |
|:---|:---|:---|
| Python | Readable, vast ML ecosystem | AI/ML, data analysis, automation |
| R | Statistics, ggplot2, bioinformatics | Statistical computing, academia |
| SQL | Universal for relational data | Database querying, aggregation |
| Scala | JVM-based, Apache Spark native | Big Data engineering with Spark |
| Julia | High-performance scientific computing | Numerical simulation, research |

### Python Library Stack (Layered)
- Data: NumPy -> Pandas -> Dask (parallel DataFrames)
- Visualization: Matplotlib -> Seaborn -> Plotly -> Dash -> Folium
- ML/AI: Scikit-learn -> XGBoost -> LightGBM -> TensorFlow -> PyTorch
- NLP: NLTK -> spaCy -> Hugging Face Transformers
- Model Serving: Flask -> FastAPI -> BentoML -> MLflow

### R Library Stack
- Data Manipulation: dplyr, tidyr, data.table
- Visualization: ggplot2, plotly, shiny
- Machine Learning: caret, tidymodels, randomForest
- Time Series: forecast, tseries

---

## Module 2: Jupyter Notebooks and JupyterLab

Architecture: Browser UI <-> Jupyter Server (Python/Tornado) <-> Kernel (IPython/R/Julia)
Notebooks are stored as .ipynb files in JSON format on disk.

### Cell Types in Jupyter
1. Code Cells - Executable Python/R/Julia code, shows inline output
2. Markdown Cells - Rich text with headers, lists, LaTeX math, images, links
3. Raw Cells - Plain unformatted text, not executed or rendered

### Essential Keyboard Shortcuts
| Shortcut | Mode | Action |
|:---|:---|:---|
| Shift+Enter | Either | Run cell and move to next |
| Ctrl+Enter | Either | Run cell in place |
| Alt+Enter | Either | Run cell and insert new below |
| Esc | Command | Enter command mode |
| A | Command | Insert cell Above current |
| B | Command | Insert cell Below current |
| D,D | Command | Delete current cell |
| M | Command | Convert cell to Markdown |
| Y | Command | Convert cell to Code |
| Z | Command | Undo last cell operation |
| Ctrl+S | Either | Save notebook |
| 0,0 | Command | Restart kernel |
| Ctrl+Shift+- | Edit | Split cell at cursor |

### Markdown Quick Reference
- Headers: # H1  ## H2  ### H3
- Formatting: **Bold**  *Italic*  inline code  ~~strikethrough~~
- Lists: - bullet  or  1. numbered
- LaTeX block: enclosed in double-dollar signs - E = mc^2
- LaTeX inline: enclosed in single-dollar signs - sigma = sqrt{Var(X)}
- Table: | Col | Col | with |---|---| separator

### JupyterLab vs Classic Notebook
| Feature | Classic Notebook | JupyterLab |
|:---|:---|:---|
| Interface | Single-panel | Multi-tab IDE layout |
| Multiple notebooks | Opens new browser tabs | Side-by-side tabs |
| Terminal | Separate browser window | Integrated terminal panel |
| Extensions | Limited | Full plugin ecosystem |
| File Browser | Basic sidebar | Full integrated sidebar |

---

## Module 3: Git and GitHub for Data Science

### Core Git Concepts
| Term | Definition |
|:---|:---|
| Repository | A project directory tracked by Git (.git/ folder) |
| Commit | A snapshot of all tracked files at a point in time |
| Branch | An independent line of development |
| Merge | Integrating changes from one branch into another |
| Remote | A server-hosted copy of the repo (e.g., on GitHub) |
| Clone | Download a full copy of a remote repo locally |
| Pull | Fetch + merge remote changes into local branch |
| Push | Upload local commits to the remote |
| Staging Area | Files marked for the next commit (via git add) |

### Essential Git Commands Reference

INITIAL SETUP:
  git config --global user.name  Pandya Shashank
  git config --global user.email your.email@example.com
  git config --global core.editor code

START A PROJECT:
  git init                             (Initialize new repo in current folder)
  git clone https://github.com/u/r    (Clone existing repo from remote)

DAILY WORKFLOW:
  git status                           (See modified/staged/untracked files)
  git add .                            (Stage ALL changes in directory)
  git add filename.py                  (Stage specific file)
  git commit -m "feat: add model"      (Commit snapshot with message)

REMOTE OPERATIONS:
  git remote add origin URL            (Link local repo to GitHub)
  git push -u origin main              (First push, sets upstream tracking)
  git push                             (Subsequent pushes)
  git pull                             (Fetch + merge latest from remote)
  git fetch origin                     (Fetch without merging)

BRANCHING:
  git branch feature/eda               (Create new branch)
  git checkout feature/eda             (Switch to branch)
  git checkout -b feature/eda          (Create + switch in one step)
  git merge feature/eda                (Merge branch into current)
  git branch -d feature/eda            (Delete merged branch)

HISTORY AND DIFF:
  git log --oneline -10                (Last 10 commits, compact format)
  git log --graph --oneline --all      (Visual branch history graph)
  git diff                             (Unstaged working directory changes)
  git diff --staged                    (Staged changes ready to commit)
  git show HASH                        (Show changes in specific commit)

UNDOING CHANGES:
  git restore filename.py              (Discard unstaged changes in file)
  git restore --staged filename.py     (Unstage a file, keep changes)
  git reset HEAD~1                     (Undo last commit, keep changes staged)
  git revert HASH                      (New commit that reverses a past commit)

### .gitignore Patterns for Data Science
- Python bytecode: __pycache__/  *.pyc  *.pyo
- Jupyter artifacts: .ipynb_checkpoints/
- Virtual environments: venv/  .env/  .venv/  env/
- Data files (large - use Git LFS): *.csv  *.xlsx  *.parquet  data/raw/
- Model files: *.pkl  *.joblib  *.h5  *.onnx  models/
- Credentials (NEVER commit!): .env  secrets.yaml  *_credentials.json
- OS artifacts: .DS_Store  Thumbs.db

### GitHub Collaboration - Fork and Pull Request Workflow
1. Fork the repository on GitHub (your personal copy)
2. git clone https://github.com/YOUR_USERNAME/forked-repo
3. git checkout -b feature/new-analysis
4. Make changes and commit
5. git push origin feature/new-analysis
6. Open Pull Request: your fork -> original upstream repo
7. Reviewer reviews, approves or requests changes
8. Maintainer merges PR into main branch

---

## Module 4: IBM Watson Studio and Cloud Tools

### Architecture
IBM Cloud -> IBM Watson Studio Project
  -> Jupyter Notebooks (Python 3.9 / R 4.x / Scala 2.x kernels)
  -> AutoAI Experiments (automated feature eng + algorithm selection)
  -> SPSS Modeler Flows (visual drag-and-drop ML canvas)
  -> Data Refinery (browser-based data cleaning and transformation)
  -> Datasets (IBM Cloud Object Storage / S3 buckets)
  -> Model Deployments (REST API endpoints via Watson Machine Learning)

### Watson Studio Asset Types
| Asset | Description | OSS Equivalent |
|:---|:---|:---|
| Notebook | Jupyter (Python/R/Scala) | Local JupyterLab |
| Dataset | File in COS (CSV, JSON, Parquet) | Local filesystem |
| AutoAI Experiment | Auto pipeline + model comparison | H2O AutoML, AutoSklearn |
| SPSS Flow | Visual ML modeling canvas | KNIME, Orange3 |
| Deployment | Live REST API for real-time scoring | Flask / FastAPI |

### IBM Cloud Object Storage - Python Access Pattern
`python
import ibm_boto3
from ibm_botocore.client import Config

cos = ibm_boto3.client(
    service_name='s3',
    ibm_api_key_id='YOUR_API_KEY',
    ibm_service_instance_id='YOUR_CRN',
    config=Config(signature_version='oauth'),
    endpoint_url='https://s3.us-south.cloud-object-storage.appdomain.cloud'
)
# Download: cos.download_file(Bucket='bucket', Key='data.csv', Filename='data.csv')
# Upload:   cos.upload_file(Filename='results.csv', Bucket='bucket', Key='results.csv')
`

### RStudio IDE - 4 Pane Layout
| Pane | Location | Purpose |
|:---|:---|:---|
| Source | Top-left | Script editor for .R and .Rmd files |
| Console | Bottom-left | Interactive R REPL for live execution |
| Environment/History | Top-right | Variables in memory + command history |
| Files/Plots/Packages | Bottom-right | File browser, plot viewer, pkg manager |

---

## Assessment and Grade Summary

| Assessment | Topics | Score |
|:---|:---|:---:|
| Graded Quiz 1 | Languages and Libraries | 100% |
| Graded Quiz 2 | Jupyter, RStudio, Git/GitHub | 100% |
| Graded Quiz 3 | IBM Watson Studio and Cloud | 100% |
| Final Assignment | Markdown Notebook (Peer Reviewed) | 100% |
| **Overall Grade** | **Full Course Completion** | **100%** |

---

## Tool Reference by Category

| Category | Open Source | Cloud/Enterprise |
|:---|:---|:---|
| Notebook IDE | Jupyter / JupyterLab | IBM Watson Studio / Google Colab |
| Data Manipulation | Pandas (Python) / dplyr (R) | Apache Spark / PySpark |
| SQL Databases | SQLite / PostgreSQL / MySQL | IBM Db2 / Snowflake / BigQuery |
| Version Control | Git / GitHub | GitHub Enterprise / GitLab / Bitbucket |
| ML Framework | Scikit-learn / PyTorch | IBM AutoAI / AWS SageMaker / Azure ML |
| Dashboard | Plotly Dash / Streamlit | IBM Cognos / Tableau / Power BI |
| Big Data | Apache Spark / Hadoop | Databricks / Google Dataproc |
| Experiment Tracking | MLflow | Weights and Biases / Comet ML |

---
IBM Data Science Professional Certificate - Pandya Shashank
