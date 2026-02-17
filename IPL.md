# IPL Win Prediction — Frontend

This repo contains a lightweight frontend for an IPL match win prediction model.

Files added
- `HTML/index.html` — main UI
- `CSS/styles.css` — styles
- `JS/app.js` — form handling, demo predictor, and HTTP fetch

How it works
- The UI collects match inputs (teams, toss, current score, etc.) and sends a POST to `/predict` by default.
- The expected response JSON should include probabilities. Example:

```json
{
  "probabilities": {
    "Chennai Super Kings (CSK)": 0.33,
    "Mumbai Indians (MI)": 0.67
  }
}
```

Quick start
1. Serve these files (static server or open `HTML/index.html`).
2. Update `JS/app.js` -> `PREDICT_URL` with your model endpoint (e.g., `http://localhost:5000/predict`).
3. Enable CORS on your backend so the browser can call it.

If you don't have a backend yet, use the "Use demo prediction" toggle — the UI will show a simple heuristic prediction.

Backend (Flask) scaffold

I added a minimal Flask backend that can load a pickle pipeline and serve POST /predict. Place your model file `pipe(1).pkl` inside `backend/models/` (create the `models` folder) or set the `MODEL_PATH` environment variable to point to it.

Quick start for backend (Windows PowerShell):

1. Create a venv and install deps:

   python -m venv .venv
   .\.venv\Scripts\activate
   pip install -r backend/requirements.txt

2. Copy your model to `backend/models/pipe(1).pkl` or set `MODEL_PATH`:

   mkdir backend\models
   copy "path\to\pipe(1).pkl" backend\models\

3. Run the server:

   python backend\app.py

The backend listens on http://127.0.0.1:5000 by default and enables CORS for local development.

Example request (curl):

  curl -X POST http://127.0.0.1:5000/predict -H "Content-Type: application/json" -d "{\"team1\":\"Chennai Super Kings (CSK)\",\"team2\":\"Mumbai Indians (MI)\",\"toss_winner\":\"Chennai Super Kings (CSK)\",\"toss_decision\":\"bat\",\"overs\":5.0,\"runs\":42,\"wickets\":1}"

Frontend connection

The frontend `JS/app.js` default `PREDICT_URL` now points to `http://127.0.0.1:5000/predict` for easier local testing. You can change it back if your model API runs elsewhere.

Run helper script (PowerShell)

A helper PowerShell script is included: `backend/run_server.ps1`. It creates a virtual environment, installs dependencies, optionally copies your model into `backend/models/pipe(1).pkl`, and starts the Flask server.

Examples (PowerShell):

- Copy your model and run the server:

  .\backend\run_server.ps1 -ModelSource "C:\path\to\pipe(1).pkl"

- Run the server if model already in `backend/models`:

  .\backend\run_server.ps1

Validate the model locally

A test script `backend/test_model.py` is included to verify your model loads and produces predictions. Run it with the same Python used in the venv:

  .\.venv\Scripts\python backend\test_model.py

If you prefer, you can also set the `MODEL_PATH` environment variable to point directly to your `pipe(1).pkl` file instead of copying it into `backend/models`.

If you'd like, I can further adapt the server to match your pipeline's exact column names and output format. ✅