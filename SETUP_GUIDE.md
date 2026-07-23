# SalesAI Setup Guide

## Quick Start

### 1. Get Your Gemini API Key

1. Go to [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Click "Create API Key"
3. Copy your API key

### 2. Configure Environment

Open `.env` file and add your Gemini API key:

```env
GEMINI_API_KEY=your_actual_api_key_here
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the Application

#### Option A: Command Line (Simple)
```bash
python main.py
```

#### Option B: Web Interface (Recommended)

**Terminal 1 - Start Backend:**
```bash
python api.py
```

**Terminal 2 - Open Frontend:**
- Simply open `frontend/index.html` in your browser
- Or use a local server:
  ```bash
  cd frontend
  python -m http.server 8080
  ```
  Then visit: http://localhost:8080

### 5. Test Email Sending

The system is currently set to **send real emails** via Gmail.

To test safely:
1. Set `DRY_RUN=true` in `.env` (emails will only print to console)
2. Or keep `DRY_RUN=false` to send real emails

---

## What Changed?

### ✅ Fixed Issues:
1. **Switched from Ollama to Gemini** - No need to run local LLM server
2. **Removed preamble text** - Emails now start directly with "Subject:" line
3. **Better error handling** - Clear error messages if API key is missing
4. **Modern frontend** - Beautiful, functional UI with working send buttons

### 🎨 Frontend Features:
- Gradient design with glass-morphism effects
- Live backend status indicator
- Two send buttons (header + main)
- Toast notifications with icons
- Smooth animations and transitions
- Responsive design

---

## Troubleshooting

### "GEMINI_API_KEY not set" Error
- Make sure you added your API key to `.env`
- Restart the backend after updating `.env`

### Backend Offline
- Check if `python api.py` is running
- Verify it's running on port 8000
- Check console for error messages

### Emails Not Sending
- Verify Gmail credentials in `.env`
- Check if 2-factor authentication is enabled (use App Password)
- Set `DRY_RUN=true` to test without sending

---

## API Endpoints

- `GET /customers` - List all customers
- `GET /customers/{email}` - Get customer details
- `POST /customers/{email}/generate` - Generate AI email
- `POST /customers/{email}/send` - Send email

---

## Project Structure

```
SalesAI/
├── agents.py           # AI agents (EmailWriter, SalesManager)
├── llm.py             # Gemini API integration
├── email_sender.py    # SMTP email sending
├── api.py             # FastAPI backend
├── main.py            # CLI interface
├── frontend/          # Web UI
│   ├── index.html
│   ├── app.js
│   └── styles.css
├── data/
│   └── customers.csv  # Lead database
└── .env               # Configuration
```

---

## Next Steps

1. Add your Gemini API key to `.env`
2. Run `python api.py`
3. Open `frontend/index.html`
4. Select a lead and click "Generate Email"
5. Review and send!

Enjoy your AI-powered sales outreach! 🚀
