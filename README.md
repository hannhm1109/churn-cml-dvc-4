# Churn Detection — MLOps Pipeline with DVC, S3, GitHub Actions and CML

End-to-end MLOps project for customer churn prediction using versioned data and automated CI/CD.

---

## Project Overview

This project trains a classification model to predict customer churn using three approaches to handle class imbalance. The full pipeline is automated via GitHub Actions and publishes a performance report on every push.

---

## Tech Stack

- Python 3.11
- scikit-learn, imbalanced-learn
- DVC 3.63.0 — data and model versioning
- AWS S3 — remote storage for artifacts
- GitHub Actions — CI/CD pipeline
- CML — automated report publishing

---

## Project Structure

```
churn-cml-dvc/
├── data/                  # Training data (tracked by DVC)
├── models/                # Trained models (tracked by DVC)
├── script.py              # Training script
├── requirements.txt       # Python dependencies
├── data.dvc               # DVC pointer for data/
├── models.dvc             # DVC pointer for models/
├── conf_matrix.png        # Confusion matrix (generated)
├── metrics.txt            # Model metrics (generated)
└── .github/
    └── workflows/
        └── dvc-churn.yaml # CI/CD pipeline
```

---

## Models and Approaches

The script trains a Logistic Regression model using three strategies to address class imbalance:

1. Without handling imbalance (baseline)
2. With class weights
3. With SMOTE oversampling

---

## CI/CD Pipeline

On every `git push`, the GitHub Actions pipeline automatically:

1. Restores data and models from S3 via `dvc pull`
2. Runs `script.py` to retrain the model
3. Versions new artifacts with DVC and pushes them to S3
4. Generates a report with metrics and confusion matrix
5. Posts the report as a comment on the commit via CML

---

## Setup

### 1. Clone the repo

```bash
git clone https://github.com/hannhm1109/churn-cml-dvc-4.git
cd churn-cml-dvc-4
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
pip install "pathspec==0.11.2"
pip install "dvc[s3]==3.63.0"
```

### 3. Configure AWS credentials

```bash
export AWS_ACCESS_KEY_ID=your_key
export AWS_SECRET_ACCESS_KEY=your_secret
export AWS_DEFAULT_REGION=eu-north-1
```

### 4. Pull data and models from S3

```bash
dvc pull
```

### 5. Run the training script

```bash
python script.py
```

---

## GitHub Secrets Required

| Secret | Description |
|--------|-------------|
| `AWS_ACCESS_KEY_ID` | IAM user access key |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret key |
| `AWS_DEFAULT_REGION` | AWS region (eu-north-1) |

---

## Author

Hanane Nahim — ENSET Mohammedia
Atelier 4 
