# SalesAI Project Report - Part 1: Introduction

## Executive Summary

**Project Name:** SalesAI - Multi-Agent Sales Email Automation System  
**Technology Stack:** Python, FastAPI, Ollama (LLM), JavaScript, Tailwind CSS  
**Purpose:** Automated AI-powered sales email generation and outreach management  
**Status:** Fully Functional

---

## 1. Project Overview

### 1.1 Problem Statement
Sales teams spend significant time crafting personalized outreach emails for potential leads. This manual process is:
- Time-consuming (30-60 minutes per email)
- Inconsistent in quality
- Difficult to scale
- Lacks data-driven decision making

### 1.2 Solution
SalesAI is an intelligent multi-agent system that:
- Automatically selects high-quality leads based on scoring criteria
- Generates multiple email drafts using different writing styles
- Uses AI to evaluate and select the best email
- Sends personalized emails via SMTP
- Provides a modern web interface for human oversight

### 1.3 Key Features
✅ **Multi-Agent Architecture** - 3 specialized email writers + 1 manager agent  
✅ **Local AI Processing** - Uses Ollama (no cloud API costs)  
✅ **Lead Scoring** - Automatic qualification based on configurable criteria  
✅ **Email Personalization** - Tailored to customer profile and industry  
✅ **Modern Web UI** - Beautiful, responsive interface with real-time feedback  
✅ **SMTP Integration** - Direct email sending via Gmail or other providers  
✅ **Dry-Run Mode** - Safe testing without sending real emails  

---

## 2. System Architecture

### 2.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INTERFACE                          │
│                    (Web Browser - Frontend)                     │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │
│  │ Lead List    │  │ Lead Details │  │ Email Editor │        │
│  │ - Filtering  │  │ - Scoring    │  │ - Generation │        │
│  │ - Selection  │  │ - History    │  │ - Sending    │        │
│  └──────────────┘  └──────────────┘  └──────────────┘        │
└─────────────────────────────────────────────────────────────────┘
                              ▲
                              │ REST API (HTTP/JSON)
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      APPLICATION LAYER                          │
│                    (FastAPI Backend - Python)                   │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │                    API Endpoints                          │ │
│  │  • GET  /customers          - List all leads             │ │
│  │  • GET  /customers/{email}  - Get lead details           │ │
│  │  • POST /customers/{email}/generate - Generate email     │ │
│  │  • POST /customers/{email}/send - Send email             │ │
│  └──────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              ▲
                              │
                    ┌─────────┴─────────┐
                    │                   │
                    ▼                   ▼
┌──────────────────────────┐  ┌──────────────────────────┐
│    BUSINESS LOGIC        │  │    INTEGRATION LAYER     │
│   (AI Agents Layer)      │  │   (Email Sender)         │
│                          │  │                          │
│  ┌────────────────────┐ │  │  ┌────────────────────┐ │
│  │ EmailWriterAgent   │ │  │  │  SMTP Client       │ │
│  │ - value_focus      │ │  │  │  - Gmail           │ │
│  │ - relationship     │ │  │  │  - Authentication  │ │
│  │ - urgency          │ │  │  │  - TLS/SSL         │ │
│  └────────────────────┘ │  │  └────────────────────┘ │
│                          │  │                          │
│  ┌────────────────────┐ │  └──────────────────────────┘
│  │ SalesManagerAgent  │ │
│  │ - Lead Selection   │ │
│  │ - Email Evaluation │ │
│  └────────────────────┘ │
└──────────────────────────┘
                ▲
                │ LLM API Calls
                ▼
┌──────────────────────────┐
│      AI/LLM LAYER        │
│   (Ollama Server)        │
│                          │
│  • llama3.2 Model        │
│  • Text Generation       │
│  • JSON Generation       │
│  • Local Processing      │
└──────────────────────────┘
                ▲
                │
                ▼
┌──────────────────────────┐
│      DATA LAYER          │
│                          │
│  • customers.csv         │
│  • Lead Database         │
│  • Configuration (.env)  │
└──────────────────────────┘
```

### 2.2 Technology Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | HTML5, JavaScript (ES6+), Tailwind CSS | User interface |
| **Backend** | Python 3.10+, FastAPI, Uvicorn | REST API server |
| **AI/LLM** | Ollama, llama3.2 | Text generation |
| **Data** | Pandas, CSV | Lead management |
| **Email** | smtplib, Gmail SMTP | Email delivery |
| **Config** | python-dotenv | Environment management |

### 2.3 Component Breakdown

#### Frontend Components
- **Lead Sidebar** - Displays all leads with filtering and selection
- **Detail Panel** - Shows selected lead information
- **Email Composer** - Displays generated email with editing capability
- **Status Indicators** - Real-time backend connection status
- **Toast Notifications** - User feedback for actions

#### Backend Components
- **API Router** - Handles HTTP requests and routing
- **Agent Manager** - Coordinates AI agents
- **LLM Interface** - Communicates with Ollama
- **Email Service** - Manages SMTP connections
- **Data Loader** - Reads and parses CSV data

#### AI Components
- **EmailWriterAgent (3 instances)** - Generate email drafts
- **SalesManagerAgent** - Select leads and evaluate emails
- **LLM Wrapper** - Abstracts Ollama API calls

---

## 3. Project Structure

```
SalesAI/
│
├── 📁 frontend/                    # Web Interface
│   ├── index.html                  # Main UI page
│   ├── app.js                      # Frontend logic
│   └── styles.css                  # Custom styling
│
├── 📁 data/                        # Data Storage
│   └── customers.csv               # Lead database
│
├── 🐍 Backend Python Files
│   ├── api.py                      # FastAPI REST API
│   ├── main.py                     # CLI interface
│   ├── agents.py                   # AI agent implementations
│   ├── llm.py                      # Ollama LLM interface
│   └── email_sender.py             # SMTP email sender
│
├── ⚙️ Configuration Files
│   ├── .env                        # Environment variables
│   ├── .env.example                # Config template
│   ├── requirements.txt            # Python dependencies
│   └── .gitignore                  # Git exclusions
│
├── 📚 Documentation
│   ├── README.md                   # Project overview
│   ├── OLLAMA_SETUP.md            # Ollama installation
│   ├── START_WITH_OLLAMA.md       # Quick start guide
│   ├── ARCHITECTURE.md             # System architecture
│   ├── UI_FEATURES.md              # Frontend features
│   └── PROJECT_REPORT.md           # This report
│
└── 🧪 Testing & Utilities
    ├── test_ollama.py              # Ollama test suite
    ├── send_test_email.py          # Email testing
    └── check_setup.py              # Setup verification
```

---

## 4. Key Metrics

### 4.1 Performance Metrics
- **Email Generation Time:** 30-60 seconds per customer
- **Lead Processing:** 3 drafts + evaluation per lead
- **API Response Time:** < 1 second (excluding LLM calls)
- **Frontend Load Time:** < 500ms

### 4.2 Capacity Metrics
- **Concurrent Users:** 1-5 (local deployment)
- **Leads Supported:** Unlimited (CSV-based)
- **Email Throughput:** 50-100 emails/hour (limited by LLM speed)

### 4.3 Quality Metrics
- **Email Personalization:** 100% (all emails customized)
- **Multi-Agent Evaluation:** 3 drafts per email
- **Human Oversight:** Required before sending

---

## 5. Development Timeline

### Phase 1: Core Development
- ✅ Backend API implementation
- ✅ AI agent architecture
- ✅ Ollama integration
- ✅ Basic frontend

### Phase 2: Enhancement (Current)
- ✅ Modern UI redesign
- ✅ Error handling improvements
- ✅ Email preamble removal
- ✅ Timeout optimization
- ✅ Documentation

### Phase 3: Future Enhancements
- ⏳ Database integration (PostgreSQL)
- ⏳ User authentication
- ⏳ Email tracking and analytics
- ⏳ A/B testing framework
- ⏳ CRM integration

---

**Continue to Part 2: Technical Implementation →**
