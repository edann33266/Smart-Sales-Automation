# SalesAI Architecture with Ollama

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         Browser                              │
│                   (frontend/index.html)                      │
│                                                              │
│  ┌────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │   Leads    │  │ Lead Details │  │    Email     │       │
│  │  Sidebar   │  │    Panel     │  │   Composer   │       │
│  └────────────┘  └──────────────┘  └──────────────┘       │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ HTTP Requests
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    FastAPI Backend                           │
│                        (api.py)                              │
│                                                              │
│  GET  /customers              → List all leads              │
│  GET  /customers/{email}      → Get lead details            │
│  POST /customers/{email}/generate → Generate email          │
│  POST /customers/{email}/send     → Send email              │
└─────────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                │                       │
                ▼                       ▼
┌──────────────────────────┐  ┌──────────────────────────┐
│      AI Agents           │  │    Email Sender          │
│     (agents.py)          │  │  (email_sender.py)       │
│                          │  │                          │
│  • EmailWriterAgent (3x) │  │  • Parse subject/body    │
│    - value_focus         │  │  • SMTP connection       │
│    - relationship_focus  │  │  • Send via Gmail        │
│    - urgency_focus       │  │  • DRY_RUN mode          │
│                          │  │                          │
│  • SalesManagerAgent     │  └──────────────────────────┘
│    - select_leads()      │              │
│    - choose_best_email() │              │
└──────────────────────────┘              │
                │                         │
                │                         ▼
                ▼                  ┌──────────────┐
┌──────────────────────────┐      │     SMTP     │
│      LLM Interface       │      │    Server    │
│        (llm.py)          │      │  (Gmail)     │
│                          │      └──────────────┘
│  • generate_text()       │
│  • generate_json()       │
│  • Error handling        │
└──────────────────────────┘
                │
                │ HTTP API Calls
                ▼
┌──────────────────────────┐
│    Ollama Server         │
│  (localhost:11434)       │
│                          │
│  • llama3.2 model        │
│  • Text generation       │
│  • JSON generation       │
└──────────────────────────┘
```

---

## 🔄 Email Generation Flow

```
1. User clicks "Generate Email" in browser
                │
                ▼
2. Frontend sends POST to /customers/{email}/generate
                │
                ▼
3. Backend (api.py) calls agents.py
                │
                ▼
4. Three EmailWriterAgents generate drafts in parallel
                │
                ├─► value_focus agent → llm.py → Ollama
                ├─► relationship_focus agent → llm.py → Ollama
                └─► urgency_focus agent → llm.py → Ollama
                │
                ▼
5. SalesManagerAgent evaluates all 3 drafts
                │
                ▼ (sends all drafts to LLM)
6. llm.py → Ollama (returns JSON with best choice)
                │
                ▼
7. Backend returns: chosen_agent, final_email, reasoning
                │
                ▼
8. Frontend displays email in textarea
                │
                ▼
9. User clicks "Send Email"
                │
                ▼
10. Backend sends via SMTP (or prints if DRY_RUN=true)
```

---

## 🧠 AI Agent Workflow

### EmailWriterAgent
```python
Input:
  • Customer profile (name, company, industry, etc.)
  • Product info (name, value proposition)
  • Style description (value/relationship/urgency focus)

Process:
  1. Build system prompt with style instructions
  2. Build user prompt with customer data
  3. Call Ollama via llm.generate_text()
  4. Clean output (remove preambles)

Output:
  • Complete email with subject line
```

### SalesManagerAgent
```python
select_leads():
  Input: List of customers
  Process: Filter by lead_score >= 80 AND last_contact >= 14 days
  Output: Selected customers

choose_best_email():
  Input: Customer profile + 3 email drafts
  Process:
    1. Build evaluation prompt
    2. Call Ollama via llm.generate_json()
    3. Parse JSON response
  Output: {chosen_agent, final_email, reasoning}
```

---

## 📁 File Structure

```
SalesAI/
├── 🌐 Frontend
│   ├── index.html          # Main UI
│   ├── app.js              # JavaScript logic
│   └── styles.css          # Styling
│
├── 🔧 Backend
│   ├── api.py              # FastAPI REST API
│   ├── main.py             # CLI interface
│   ├── agents.py           # AI agents
│   ├── llm.py              # Ollama interface
│   └── email_sender.py     # SMTP email sending
│
├── 📊 Data
│   └── data/customers.csv  # Lead database
│
├── ⚙️ Configuration
│   ├── .env                # Environment variables
│   ├── .env.example        # Config template
│   └── requirements.txt    # Python dependencies
│
└── 📚 Documentation
    ├── OLLAMA_SETUP.md     # Ollama installation guide
    ├── START_WITH_OLLAMA.md # Quick start
    ├── ARCHITECTURE.md     # This file
    ├── UI_FEATURES.md      # Frontend features
    └── test_ollama.py      # Test script
```

---

## 🔌 API Endpoints

### GET /customers
Returns all customers with selection flags
```json
[
  {
    "name": "Atharv Bhardwaj",
    "email": "atharvbhardwaj07@gmail.com",
    "company": "Acme Analytics",
    "lead_score": 92,
    "selected_for_outreach": true
  }
]
```

### POST /customers/{email}/generate
Generates email for specific customer
```json
{
  "customer": {...},
  "chosen_agent": "value_focus",
  "final_email": "Subject: ...\n\nDear Atharv,...",
  "reasoning": "Chose value_focus because..."
}
```

### POST /customers/{email}/send
Sends email to customer
```json
Request: {"email_text": "Subject: ...\n\n..."}
Response: {"status": "ok"}
```

---

## 🔄 Data Flow

```
CSV File → pandas → Customer objects → SalesManager → Selected leads
                                              │
                                              ▼
                                    EmailWriterAgents (3x)
                                              │
                                              ▼
                                         Ollama LLM
                                              │
                                              ▼
                                    Email drafts (3x)
                                              │
                                              ▼
                                      SalesManager
                                              │
                                              ▼
                                         Ollama LLM
                                              │
                                              ▼
                                       Best email
                                              │
                                              ▼
                                       SMTP Sender
                                              │
                                              ▼
                                      Gmail / Console
```

---

## 🎯 Key Components

### 1. Ollama Server
- **Purpose:** Local LLM inference
- **Port:** 11434
- **Models:** llama3.2, llama3.1, etc.
- **API:** HTTP REST API

### 2. FastAPI Backend
- **Purpose:** REST API for frontend
- **Port:** 8000
- **Framework:** FastAPI + Uvicorn
- **CORS:** Enabled for local development

### 3. Frontend
- **Purpose:** User interface
- **Tech:** Vanilla JS + Tailwind CSS
- **Features:** Lead selection, email generation, sending

### 4. AI Agents
- **Purpose:** Generate and evaluate emails
- **Types:** 3 writers + 1 manager
- **LLM:** Ollama (local)

### 5. Email Sender
- **Purpose:** Send emails via SMTP
- **Provider:** Gmail (configurable)
- **Safety:** DRY_RUN mode for testing

---

## 🔐 Security Considerations

1. **API Keys:** Gmail password in .env (use App Password)
2. **CORS:** Currently allows all origins (dev only)
3. **Rate Limiting:** None (add for production)
4. **Input Validation:** Basic (enhance for production)
5. **DRY_RUN:** Enabled by default for safety

---

## 📊 Performance

### Typical Response Times (with llama3.2)
- **Email generation:** 10-30 seconds per draft
- **Manager evaluation:** 15-40 seconds
- **Total per customer:** 45-120 seconds

### Optimization Tips
1. Use smaller model (llama3.2:1b) for faster responses
2. Increase RAM allocation for Ollama
3. Use GPU if available
4. Cache common responses

---

## 🚀 Deployment Considerations

### Local Development (Current)
- Ollama on localhost
- FastAPI on localhost:8000
- Frontend as static files

### Production (Future)
- Ollama on dedicated server
- FastAPI behind reverse proxy (nginx)
- Frontend on CDN
- Add authentication
- Add rate limiting
- Use production SMTP service
- Add logging and monitoring

---

This architecture provides a solid foundation for AI-powered sales automation! 🎉
