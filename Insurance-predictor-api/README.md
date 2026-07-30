# Backend — FastAPI ML Service

This directory contains the prediction API, training data, experiment notebook, and trained model for the Insurance Premium Category Predictor.

## Stack

- FastAPI and Pydantic for the web API and request validation
- scikit-learn for the trained prediction model
- pandas for building model input features

## Files

| File | Purpose |
| --- | --- |
| `app.py` | Defines the FastAPI application and `/predict` endpoint |
| `model.pkl` | Serialized trained machine-learning model loaded at startup |
| `insurance.csv` | Dataset used during experimentation/training |
| `fastapi_ml_model.ipynb` | Notebook documenting model development |
| `requirements.txt` | Python dependencies |

## Setup and run

`model.pkl` is loaded from the same directory as `app.py`, so the service can be started from this directory or from the repository root.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn app:app --reload
```

The server runs at `http://127.0.0.1:8000`. Explore and try the API at `http://127.0.0.1:8000/docs`.

## Prediction endpoint

`POST /predict`

Example request:

```json
{
  "age": 30,
  "weight": 65,
  "height": 1.7,
  "income_lpa": 10,
  "smoker": false,
  "city": "Mumbai",
  "occupation": "private_job"
}
```

Validation rules include a positive age below 120, positive height/weight/income, and one of these occupation values:

```text
retired, freelancer, student, government_job, business_owner, unemployed, private_job
```

## Feature engineering

Before prediction, the API derives the following fields:

- **BMI:** `weight / height²`
- **Age group:** young, adult, middle-aged, or senior
- **Lifestyle risk:** based on smoking status and BMI
- **City tier:** tier 1, 2, or 3 according to the city lists in `app.py`

These engineered fields, together with income and occupation, are passed to the saved model. A successful response is:

```json
{ "predicted_category": "..." }
```

## Frontend integration

The Streamlit frontend should send requests to:

```text
http://127.0.0.1:8000/predict
```

### CORS configuration

CORS middleware is configured for Streamlit's local development origins:

```text
http://localhost:8501
http://127.0.0.1:8501
```

When deploying a browser-based frontend on another domain, add its exact URL to `allowed_origins` in `app.py`. For example:

```python
allowed_origins = [
    "https://your-frontend.example.com",
]
```

Avoid using `"*"` for `allow_origins` in production, especially when credentials are enabled.
