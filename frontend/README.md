# Frontend — Streamlit App

This directory contains the user interface for the Insurance Premium Category Predictor. The app collects an applicant's details, calls the FastAPI backend, and displays the returned premium category.

## Stack

- [Streamlit](https://streamlit.io/) for the interface
- `requests` for API communication

## Setup

From this directory:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Configure the backend URL

Open `frontend.py` and set `API_URL` to the backend prediction endpoint:

```python
API_URL = "http://127.0.0.1:8000/predict"
```

For deployment, replace this with the public URL of your backend, including `/predict`.

## Run

```powershell
streamlit run frontend.py
```

Streamlit will print a local URL, normally `http://localhost:8501`.

## Inputs sent to the API

| Field | Description |
| --- | --- |
| `age` | Applicant age in years |
| `weight` | Weight in kilograms |
| `height` | Height in metres |
| `income_lpa` | Annual income in lakhs per annum (LPA) |
| `smoker` | Smoking status |
| `city` | Applicant city |
| `occupation` | One of the supported occupation values |

The UI expects a successful response in this shape:

```json
{ "predicted_category": "..." }
```

For API details, see the [backend README](../Insurance-predictor-api/README.md).

