# Heart Disease Risk Prediction — End-to-End MLOps

**Course:** BITS Pilani AIMLCZG523 — Assignment 01

**Repository:** https://github.com/2025ae05964/mlops-heart-disease-prediction

## 1. Project Overview

This project implements an end-to-end MLOps pipeline to predict the presence or absence of heart disease using patient health data.

The workflow includes:
- Dataset preparation and exploratory data analysis (EDA)
- Logistic Regression and Random Forest model training
- Model evaluation and MLflow experiment tracking
- Automated testing and code quality checks
- FastAPI model-serving API
- Docker containerization and Kubernetes deployment
- Prometheus metrics and Grafana monitoring
- GitHub Actions CI/CD workflow

This project is for educational purposes. Predictions are not a medical diagnosis.

## 2. Setup and Dataset

The Cleveland heart-disease dataset was prepared locally.

- Dataset file: `data/heart_raw.csv`
- Dataset size: **303 rows × 14 columns**
- EDA figures: `reports/figures/`

To install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## 3. Exploratory Data Analysis

The target distribution was:

| Target | Count |
|---|---:|
| 0 — Absence of heart disease | 164 |
| 1 — Presence of heart disease | 139 |
| Total | 303 |

The classes are relatively close in size, but not exactly balanced. Histograms, a correlation heatmap, and a class-balance chart were generated for further analysis.

Figures:
- `screenshots/ss-05-eda-histograms.png`
- `screenshots/ss-06-eda-correlation.png`
- `screenshots/ss-07-eda-class-balance.png`

## 4. Model Training and Evaluation

Two classification models were evaluated: Logistic Regression and Random Forest.

| Metric | Logistic Regression | Random Forest |
|---|---:|---:|
| Accuracy | 0.8852 | 0.8689 |
| Precision | 0.8387 | 0.8333 |
| Recall | 0.9286 | 0.8929 |
| ROC-AUC | 0.9654 | 0.9481 |
| Mean CV Accuracy | 0.8346 | 0.8056 |

Logistic Regression performed better on the reported metrics and was selected as the best-performing model in this experiment. The dataset is small, so these results do not establish clinical suitability.
tail -n 12 reports/final_report.md
## 5. MLflow Experiment Tracking

The MLflow experiment is named `heart-disease-classification`.

Two successful experiment runs were recorded:
- Logistic Regression
- Random Forest

The training workflow logs model evaluation metrics. The trained model is saved at `models/model.joblib`.

To launch the MLflow UI:

```bash
mlflow ui
## 6. Testing and Code Quality

The following commands were executed successfully:

```bash
pytest -q
flake8 src app tests --max-line-length=100
```

**Test result:** 3 tests passed. Flake8 completed without reported violations.

Evidence: `screenshots/ss-04-tests-flake8.png`
## 7. API and Docker

The FastAPI application exposes the following endpoints:

| Endpoint | Purpose |
|---|---|
| `GET /health` | Checks service health and model loading |
| `POST /predict` | Returns a prediction from patient features |
| `GET /metrics` | Exposes Prometheus monitoring metrics |

The Docker image was built successfully using:

```bash
docker build -t heart-api:latest .
```

## 8. Kubernetes Deployment

The application was deployed to a local Minikube Kubernetes cluster. Two API pods reached the Running state. The service was accessed locally using port forwarding.

Health check: `GET /health` returned `{"status":"ok","model_loaded":true}`.

## 9. Monitoring

Prometheus was configured to scrape API metrics, and the scrape target reported healthy. Grafana was configured with Prometheus as its data source. The dashboard includes request rate, total requests, p95 response time, and API status.

Evidence: `screenshots/ss-09-grafana-dashboard.png`

## 10. CI/CD

GitHub Actions workflow runs completed successfully, as shown in the repository Actions tab. The workflow covers code quality checks, tests, and model training.

## 11. Architecture

Data preparation → EDA → model training and evaluation → MLflow tracking → saved model → FastAPI → Docker → Kubernetes → Prometheus and Grafana.

## 12. Conclusion

The assignment demonstrates an end-to-end educational MLOps workflow, including model development, experiment tracking, API serving, containerization, Kubernetes deployment, monitoring, testing, and CI/CD. The reported model metrics are experimental and should not be interpreted as evidence of clinical performance.
