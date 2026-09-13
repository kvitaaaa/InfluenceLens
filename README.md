<p align="center">
  <h1 align="center">🔍 InfluenceLens</h1>
  <p align="center">
    <strong>AI-Powered Sentiment Analysis & Social Feedback Analytics Engine</strong>
  </p>
  <p align="center">
    An end-to-end MLOps system for real-time sentiment classification, trend analysis, and visual feedback summarization of user comments.
  </p>
  <p align="center">
    <a href="#features">Features</a> •
    <a href="#architecture">Architecture</a> •
    <a href="#tech-stack">Tech Stack</a> •
    <a href="#getting-started">Getting Started</a> •
    <a href="#api-reference">API Reference</a> •
    <a href="#ml-pipeline">ML Pipeline</a> •
    <a href="#cicd--deployment">CI/CD</a>
  </p>
</p>

---

## 📌 Overview

Content creators, digital marketers, and brand managers face an overwhelming volume of viewer comments across videos and campaigns. Manually reading, classifying, and quantifying this feedback is infeasible at scale.

**InfluenceLens** solves this by providing:
- 🎯 **Automatic sentiment classification** of comments into **Positive**, **Neutral**, and **Negative** categories
- 📈 **Monthly trend analysis** to track sentiment shifts over time
- 📊 **Visual summaries** including sentiment distribution charts and keyword word clouds
- 🔄 **Complete MLOps lifecycle** — from data versioning to automated cloud deployment

---

## ✨ Features

| Feature | Description |
|:---|:---|
| **Multi-Class Sentiment Analysis** | Classifies comments as Positive (`1`), Neutral (`0`), or Negative (`-1`) using LightGBM |
| **Bulk Prediction API** | Process hundreds of comments in a single API call |
| **Sentiment Pie Charts** | Dynamic pie chart generation showing sentiment distribution |
| **Word Cloud Generation** | Visual keyword summaries from comment text |
| **Monthly Trend Graphs** | Time-series visualization of sentiment shifts across months |
| **MLflow Experiment Tracking** | Full experiment logging with metrics, parameters, and model versioning |
| **DVC Data Versioning** | Reproducible data pipelines with AWS S3 storage |
| **Automated CI/CD** | GitHub Actions pipeline with testing gates and AWS deployment |
| **Docker Containerization** | Production-ready containerized deployment |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         InfluenceLens Pipeline                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐  │
│  │   Raw Data   │───▶│  Data Ingestion  │───▶│  Preprocessing   │  │
│  │  (CSV/API)   │    │  (Train/Test     │    │  (Clean, Lemma,  │  │
│  └──────────────┘    │   Split: 75/25)  │    │   TF-IDF)        │  │
│                      └──────────────────┘    └────────┬─────────┘  │
│                                                       │            │
│  ┌──────────────┐    ┌──────────────────┐    ┌────────▼─────────┐  │
│  │   MLflow     │◀───│  Model           │◀───│  Model Building  │  │
│  │   Registry   │    │  Evaluation      │    │  (LightGBM)      │  │
│  │  (Staging →  │    │  (Metrics +      │    │                  │  │
│  │  Production) │    │   Confusion Mat) │    │                  │  │
│  └──────┬───────┘    └──────────────────┘    └──────────────────┘  │
│         │                                                          │
├─────────▼──────────────────────────────────────────────────────────┤
│                       CI/CD & Deployment                           │
│  ┌──────────────┐    ┌──────────────────┐    ┌──────────────────┐  │
│  │  Pytest      │───▶│  Docker Build    │───▶│  AWS CodeDeploy  │  │
│  │  Quality     │    │  + Push to ECR   │    │  to EC2          │  │
│  │  Gates       │    │                  │    │  (Port 80)       │  │
│  └──────────────┘    └──────────────────┘    └──────────────────┘  │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │              Flask REST API (7 Endpoints)                    │   │
│  │   /predict  /predict_with_timestamps  /generate_chart       │   │
│  │   /generate_wordcloud  /generate_trend_graph  /health       │   │
│  └─────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Category | Technologies |
|:---|:---|
| **Machine Learning** | LightGBM, Scikit-Learn, NLTK (WordNet Lemmatizer) |
| **Data Processing** | Pandas, NumPy, TF-IDF Vectorization |
| **Web Framework** | Flask, Flask-CORS |
| **Visualization** | Matplotlib, Seaborn, WordCloud |
| **MLOps** | DVC (Data Version Control), MLflow (Tracking + Registry) |
| **Testing** | Pytest, Flake8 |
| **Cloud (AWS)** | S3, EC2, ECR, CodeDeploy |
| **DevOps** | Docker, GitHub Actions |

---

## 📁 Project Structure

```
InfluenceLens/
├── src/
│   ├── data/
│   │   ├── data_ingestion.py          # Data download, cleaning & train-test split
│   │   └── data_preprocessing.py      # Text normalization, stopwords & lemmatization
│   └── model/
│       ├── model_building.py          # TF-IDF vectorization & LightGBM training
│       ├── model_evaluation.py        # Metrics computation & MLflow logging
│       └── register_model.py          # MLflow model registry (Staging)
├── flask_app/
│   └── app.py                         # REST API with 7 endpoints
├── scripts/
│   ├── test_load_model.py             # Model loading validation
│   ├── test_model_signature.py        # Schema & signature verification
│   ├── test_model_performance.py      # Performance threshold gates
│   ├── test_flask_api.py              # API integration tests
│   └── promote_model.py              # Staging → Production promotion
├── deploy/
│   └── scripts/
│       ├── install_dependencies.sh    # EC2 Docker & AWS CLI setup
│       └── start_docker.sh            # Container deployment on EC2
├── .github/workflows/
│   └── cicd.yaml                      # Full CI/CD pipeline
├── dvc.yaml                           # DVC pipeline stage definitions
├── dvc.lock                           # DVC pipeline lock file
├── params.yaml                        # Centralized hyperparameters
├── Dockerfile                         # Container definition (Python 3.10-slim)
├── appspec.yml                        # AWS CodeDeploy specification
├── Makefile                           # Utility commands
├── requirements.txt                   # Python dependencies
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- Git & DVC
- AWS CLI (configured with credentials)

### Installation

```bash
# Clone the repository
git clone https://github.com/kvitaaaa/InfluenceLens.git
cd InfluenceLens

# Create and activate virtual environment
python -m venv myenv

# Windows
myenv\Scripts\activate

# Linux/macOS
source myenv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Download required NLTK data
python -m nltk.downloader stopwords wordnet
```

### Run the ML Pipeline

```bash
# Execute the complete pipeline (Ingestion → Preprocessing → Training → Evaluation → Registration)
dvc repro

# Push data and model artifacts to remote S3 storage
dvc push
```

### Run the Flask App Locally

```bash
python flask_app/app.py
# Server starts at http://localhost:5000
```

### Run with Docker

```bash
docker build -t influencelens .
docker run -p 5000:5000 influencelens
```

---

## 📡 API Reference

### `GET /health`
Health check endpoint for load balancers and monitoring.

```json
// Response
{ "status": "healthy" }
```

### `POST /predict`
Classify sentiment for a batch of comments.

```json
// Request
{ "comments": ["Great video!", "This is terrible", "Okay I guess"] }

// Response
[
  { "comment": "Great video!", "sentiment": "1" },
  { "comment": "This is terrible", "sentiment": "-1" },
  { "comment": "Okay I guess", "sentiment": "0" }
]
```

### `POST /predict_with_timestamps`
Sentiment prediction with timestamp preservation.

```json
// Request
{
  "comments": [
    { "text": "Amazing content!", "timestamp": "2024-10-01" },
    { "text": "Not helpful at all", "timestamp": "2024-10-15" }
  ]
}
```

### `POST /generate_chart`
Generate a sentiment distribution pie chart (returns `image/png`).

```json
// Request
{ "sentiment_counts": { "1": 45, "0": 30, "-1": 25 } }
```

### `POST /generate_wordcloud`
Generate a keyword word cloud from comments (returns `image/png`).

```json
// Request
{ "comments": ["Great tutorial", "Very helpful content", "Love the explanation"] }
```

### `POST /generate_trend_graph`
Generate monthly sentiment trend graph (returns `image/png`).

```json
// Request
{
  "sentiment_data": [
    { "sentiment": 1, "timestamp": "2024-10-01" },
    { "sentiment": -1, "timestamp": "2024-11-15" }
  ]
}
```

---

## 🔬 ML Pipeline

### Data Preprocessing
- **Text normalization**: Lowercasing, whitespace cleanup, special character removal
- **Smart stopword filtering**: Preserves sentiment-critical modifiers (`not`, `but`, `however`, `no`, `yet`) to prevent polarity inversion
- **Lemmatization**: NLTK WordNet lemmatizer for root word extraction

### Feature Engineering
- **TF-IDF Vectorization** with n-gram range `(1, 3)` capturing unigrams, bigrams, and trigrams
- Maximum vocabulary size: 10,000 features

### Model
- **LightGBM Classifier** optimized for multi-class sentiment classification
- Key hyperparameters (configured in `params.yaml`):
  - `learning_rate: 0.08`
  - `max_depth: 20`
  - `n_estimators: 367`
  - `objective: multiclass` with `num_class: 3`
  - Built-in class imbalance handling (`is_unbalance=True`, `class_weight='balanced'`)
  - L1/L2 regularization (`reg_alpha=0.1`, `reg_lambda=0.1`)

### Model Validation Gates
Models must pass all of the following before production promotion:
- ✅ Successful load from MLflow staging registry
- ✅ Schema and signature compatibility check
- ✅ Accuracy ≥ 0.40
- ✅ Precision ≥ 0.40
- ✅ Recall ≥ 0.40
- ✅ F1-Score ≥ 0.40

---

## 🔄 CI/CD & Deployment

The GitHub Actions pipeline (`.github/workflows/cicd.yaml`) automates the entire workflow on every push:

1. **Environment Setup** — Python 3.10 with cached pip dependencies
2. **Pipeline Execution** — `dvc repro` runs the complete ML pipeline
3. **Data Sync** — `dvc push` syncs artifacts to AWS S3
4. **Quality Gates** — Pytest validates model loading, signature, and performance
5. **Model Promotion** — Staging model promoted to Production in MLflow
6. **API Testing** — Flask server integration tests
7. **Container Build** — Docker image built and pushed to AWS ECR
8. **Cloud Deployment** — AWS CodeDeploy triggers EC2 deployment (Port 80)

---

## 🧪 Running Tests

```bash
# Model validation tests
pytest scripts/test_load_model.py
pytest scripts/test_model_signature.py
pytest scripts/test_model_performance.py

# API integration tests (requires running Flask app)
pytest scripts/test_flask_api.py
```

---

## 📄 License

This project is licensed under the terms included in the [LICENSE](LICENSE) file.

---

<p align="center">
  Built with ❤️ by <a href="https://github.com/kvitaaaa">kvitaaaa</a>
</p>
