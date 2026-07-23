# 🚀 Quick Start with Ollama

## Step-by-Step Setup (5 minutes)

### 1️⃣ Install Ollama
Download and install from: **https://ollama.com/download/windows**

### 2️⃣ Download AI Model
Open Command Prompt or PowerShell:
```bash
ollama pull llama3.2
```
⏱️ This takes 5-10 minutes (downloads ~2GB)

### 3️⃣ Start Ollama Server
```bash
ollama serve
```
✅ Keep this terminal open!

### 4️⃣ Test Ollama (Optional but Recommended)
Open a NEW terminal:
```bash
python test_ollama.py
```
This verifies everything is working.

### 5️⃣ Run SalesAI
Open ANOTHER new terminal:
```bash
python api.py
```

### 6️⃣ Open Frontend
Open `frontend/index.html` in your browser

---

## 🎯 Summary

**Terminal 1:** `ollama serve` (keep running)  
**Terminal 2:** `python api.py` (keep running)  
**Browser:** Open `frontend/index.html`

---

## ⚠️ Troubleshooting

### "Cannot connect to Ollama"
→ Make sure `ollama serve` is running in Terminal 1

### "Model not found"
→ Run: `ollama pull llama3.2`

### "Request timed out"
→ Use smaller model: `ollama pull llama3.2:1b`  
→ Or increase timeout in `.env`: `OLLAMA_TIMEOUT_S=300`

### Ollama is too slow
→ Use faster model: `ollama pull llama3.2:1b`  
→ Close other applications to free RAM

---

## 📊 Model Options

| Model | Speed | Quality | RAM |
|-------|-------|---------|-----|
| llama3.2:1b | ⚡⚡⚡ | ⭐⭐ | 2GB |
| llama3.2 | ⚡⚡ | ⭐⭐⭐ | 4GB |
| llama3.1 | ⚡ | ⭐⭐⭐⭐ | 8GB |

**Recommended:** `llama3.2` (good balance)

---

## ✅ You're Ready!

Once you see:
- Terminal 1: Ollama server running
- Terminal 2: "Uvicorn running on http://0.0.0.0:8000"
- Browser: Beautiful SalesAI interface

You can start generating AI-powered sales emails! 🎉

---

## 📚 More Help

- **Full Setup Guide:** `OLLAMA_SETUP.md`
- **Test Ollama:** `python test_ollama.py`
- **Frontend Features:** `UI_FEATURES.md`
