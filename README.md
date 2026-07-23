## SalesAI – Multi‑Agent Sales Email Demo

This project is a small **agentic sales manager demo**:

- **Lead selector (Sales Manager agent)** chooses which customers from a **dummy CSV dataset** should be contacted.
- **Three email‑writer agents** each draft a different style of email.
- The **Sales Manager agent** evaluates all drafts, selects (and optionally edits) the best one, and
- A **free SMTP email sender** (e.g. Gmail) sends the chosen email, or prints it in the console in dry‑run mode.

Everything runs locally as a simple Python script with a modern web interface.

### 🚀 Quick Start

1. **Get your Gemini API key**: https://aistudio.google.com/app/apikey
2. **Add it to `.env`**: `GEMINI_API_KEY=your_key_here`
3. **Install dependencies**: `pip install -r requirements.txt`
4. **Check setup**: `python check_setup.py`
5. **Run backend**: `python api.py`
6. **Open frontend**: Open `frontend/index.html` in your browser

### 1. Setup

- **Python**: 3.10+ recommended.

```bash
cd SalesAI
python -m venv .venv
.venv\Scripts\activate  # on Windows
pip install -r requirements.txt
```

### 2. Configure environment

Edit `.env` and fill in the values:

- **LLM** (for agents):
  - `GEMINI_API_KEY` – your Google Gemini API key (required).
  - Get it here: https://aistudio.google.com/app/apikey
  
- **Email sending (free SMTP)**:
  - `SMTP_HOST`, `SMTP_PORT` – e.g. Gmail: `smtp.gmail.com`, `587`.
  - `SMTP_USERNAME`, `SMTP_PASSWORD` – your email + password/app‑password.
  - `SMTP_FROM_NAME` – display name, e.g. `Sales AI`.
  - `DRY_RUN=true` – keeps emails from actually sending (prints to console only).

You can also use a **local SMTP debug server** (recommended for development):

- Run the included server on `127.0.0.1:1025`
- Set:
  - `SMTP_HOST=127.0.0.1`
  - `SMTP_PORT=1025`
  - `SMTP_USE_STARTTLS=false`
  - `SMTP_USE_AUTH=false`
  - `DRY_RUN=false`

You can also customize:

- `PRODUCT_NAME` – defaults to `SalesAI Copilot`.
- `PRODUCT_VALUE_PROP` – one‑sentence value proposition used in prompts.

> For safety, keep `DRY_RUN=true` while testing so no real emails are sent.

### 3. Dummy dataset

The demo uses `data/customers.csv`:

- Includes fields like `name`, `email`, `company`, `industry`, `lead_score`, `last_contact_days_ago`, etc.
- The **Sales Manager agent** currently selects leads where:
  - `lead_score >= 80` and
  - `last_contact_days_ago >= 14`.

You can edit this CSV or the heuristic in `agents.py` to change who gets contacted.

### 4. How the agents work

- **EmailWriterAgent (3 instances)**:
  - `value_focus` – emphasizes ROI and numbers.
  - `relationship_focus` – consultative, partnership‑oriented tone.
  - `urgency_focus` – short, direct, with gentle urgency.
  - Each gets its own **system prompt** and generates a full email (subject + body).

- **SalesManagerAgent**:
  - `select_leads()` – picks which customers to contact from the CSV.
  - `choose_best_email()` – sends all three drafts plus the customer profile to the LLM.
  - The LLM returns JSON with:
    - `chosen_agent`
    - `final_email` (may be lightly edited)
    - `reasoning`

- **Email sender**:
  - In `DRY_RUN` mode, just prints what would be sent.
  - When `DRY_RUN=false`, it sends real emails via SMTP.

### 5. Run the demo

```bash
.venv\Scripts\activate  # if not already active
python main.py
```

### 6. Run with local SMTP (Windows)

Open **two** terminals.

Terminal A (start the local SMTP server):

```bash
.venv\Scripts\activate
python local_smtp_server.py
```

Terminal B (run the app and send emails to the local server):

```bash
.venv\Scripts\activate
python main.py
```

You should see received emails printed in Terminal A.

You’ll see, for each selected customer:

- Which agents wrote drafts,
- Which agent the manager selected and why,
- The email that would be (or is) sent.

### 6. Extending the demo

- Add more **writer agents** with different styles by creating more `EmailWriterAgent` instances.
- Change the **lead selection** logic in `SalesManagerAgent.select_leads`.
- Log decisions to a database or dashboard instead of just printing.
- Integrate with a CRM (HubSpot, Salesforce) to fetch/update leads.

