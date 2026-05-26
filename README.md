# PulmoGuard AI Diagnostic Platform

PulmoGuard is a Flask-based lung cancer risk assessment platform that combines a trained machine learning model with user authentication, prediction history, administrative dashboards, and explainable AI output.

The application is designed for structured clinical-style workflows: users can create an account, submit symptom and risk-factor inputs, receive a risk prediction, review previous scans, and export their history. Administrators can view aggregate scan records, monitor prediction activity, and access model training tools.

Live app:

```text
https://pulmoguard-ai-diagnostic-platform.onrender.com
```

## Features

- Lung cancer risk prediction using trained Scikit-Learn/XGBoost model artifacts.
- Symptom and risk-factor questionnaire for authenticated users.
- Prediction history with timestamp, probability score, model version, and CSV export.
- Admin dashboard for reviewing user scan records and downloading all scan history.
- Explainability support through SHAP-based prediction details.
- Model training workflow with generated run reports for admin users.
- Contact form support through SMTP environment variables.
- PostgreSQL persistence for users and prediction records.
- Docker Compose setup for local app and database development.

## Tech Stack

- Python
- Flask
- Flask-Login
- PostgreSQL
- psycopg2
- Scikit-Learn
- XGBoost
- SHAP
- Pandas
- NumPy
- Docker
- Render
- Neon Postgres

## Project Structure

```
.
├── app.py
├── data/
├── static/
│   ├── css/
│   ├── images/
│   └── js/
├── templates/
├── train_model/
├── trained_models/
├── Dockerfile
├── docker-compose.yaml
├── render.yaml
└── requirements.txt
```

## Environment Variables

Copy `.env.example` to `.env` for local development and update the values for your machine:

```bash
copy .env.example .env
```

For hosted deployments, use `DATABASE_URL` instead of the individual `PG_*` variables:

```env
DATABASE_URL=postgresql://user:password@host/database?sslmode=require
SECRET_KEY=replace-with-a-production-secret
```

## Local Setup

Create and activate a virtual environment:

```bash
python -m venv .venv
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the application:

```bash
python app.py
```

Open the app at:

```text
http://127.0.0.1:5000
```

## Docker Setup

Start the Flask app and PostgreSQL database:

```bash
docker compose up --build
```

Open the app at:

```text
http://127.0.0.1:5000
```

## Deployment

This repository includes `render.yaml` for Render deployment.

Render service settings:

```text
Runtime: Python
Build Command: pip install -r requirements.txt
Start Command: gunicorn app:app
```

Required production environment variables:

```text
DATABASE_URL
SECRET_KEY
```

Required for the chatbot:

```text
GEMINI_API_KEY
GEMINI_MODEL
```

Optional contact form variables:

```text
GMAIL_USER
GMAIL_PASS
CONTACT_RECEIVER
```

Optional startup users:

```text
DEMO_USER_NAME
DEMO_USER_EMAIL
DEMO_USER_PASSWORD
DEMO_ADMIN_NAME
DEMO_ADMIN_EMAIL
DEMO_ADMIN_PASSWORD
```

The production database can be hosted on Neon Postgres. The app automatically creates the required `users` and `predictions` tables on startup.

## Demo Accounts

When the startup user variables are configured, the app creates or updates these accounts automatically:

| Role | Email | Password |
| --- | --- | --- |
| User | `user@pulmoguard.local` | `User@12345` |
| Admin | `admin@pulmoguard.local` | `Admin@12345` |

The user account can run predictions and view its own scan history. The admin account can access the admin dashboard, model training page, and all scan records.

## Health Check

```text
GET /health
```

Expected response:

```json
{
  "status": "ok"
}
```

## Important Note

PulmoGuard is a portfolio and educational diagnostic-support project. It is not a medical device and must not be used as a replacement for professional diagnosis, treatment, or clinical judgment.
