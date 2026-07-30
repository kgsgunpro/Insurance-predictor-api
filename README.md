# Insurance Premium Category Predictor

A full-stack machine-learning project that predicts an insurance premium category from an applicant's profile. It is designed as a compact end-to-end demonstration: a Streamlit interface collects data, a FastAPI service validates and enriches it, and a saved machine-learning model returns the prediction.

## Live demo

- **Web application:** [insurancepredicto.streamlit.app](https://insurancepredicto.streamlit.app/)
- **API documentation:** [FastAPI Swagger UI](https://insurance-predictor-api.fastapicloud.dev/docs)

These public deployments are provided for demonstration. The project can also be run locally using the instructions below.

## What it demonstrates

- A simple, user-friendly ML prediction interface
- A FastAPI prediction endpoint with Pydantic validation
- Feature engineering for BMI, age group, lifestyle risk, and city tier
- Model serving with scikit-learn and pandas
- A frontend and backend that can be deployed independently

## Project structure

```text
.
├── frontend/                    # Streamlit web application
│   ├── frontend.py
│   └── README.md
└── Insurance-predictor-api/     # FastAPI service and ML assets
    ├── app.py
    ├── model.pkl
    ├── insurance.csv
    ├── fastapi_ml_model.ipynb
    └── README.md
```

## How it works

1. A user enters age, height, weight, income, smoking status, city, and occupation.
2. The Streamlit application sends the data to the backend's `POST /predict` endpoint.
3. The API validates the request and derives model features such as BMI and lifestyle risk.
4. The trained model returns a premium category, which is shown in the interface.

## Run locally

Use two terminals. The commands below assume PowerShell on Windows, but work similarly on macOS/Linux with the relevant virtual-environment activation command.

### 1. Start the backend

```powershell
cd Insurance-predictor-api
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn app:app --reload
```

The API will be available at `http://127.0.0.1:8000`; its interactive documentation is at `http://127.0.0.1:8000/docs`.

### 2. Configure and start the frontend

In `frontend/frontend.py`, set the API address for a local demo:

```python
API_URL = "http://127.0.0.1:8000/predict"
```

Then, in a second terminal:

```powershell
cd frontend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
streamlit run frontend.py
```

## API example

```http
POST /predict
Content-Type: application/json
```

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

The service responds with:

```json
{
  "predicted_category": "..."
}
```

See the component-level READMEs for setup and implementation details: [frontend](frontend/README.md) and [backend](Insurance-predictor-api/README.md).

## Notes for deployment

- Deploy the FastAPI backend first and replace `API_URL` in the frontend with the deployed `/predict` URL.
- The API permits Streamlit's standard local development origins. Add your deployed frontend URL to `allowed_origins` in `app.py` before production deployment.
- Keep `model.pkl` alongside `app.py`; the API resolves the model path relative to the application file when it starts.
