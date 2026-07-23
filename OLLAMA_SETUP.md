# Ollama Setup Guide for SalesAI

## 📥 Step 1: Install Ollama

### Windows
1. Download Ollama from: https://ollama.com/download/windows
2. Run the installer (OllamaSetup.exe)
3. Follow the installation wizard
4. Ollama will start automatically as a background service

### Verify Installation
Open Command Prompt or PowerShell and run:
```bash
ollama --version
```

You should see something like: `ollama version is 0.x.x`

---

## 🤖 Step 2: Download a Model

Ollama needs to download the AI model first. I recommend **llama3.2** (smaller and faster than llama3.1):

```bash
ollama pull llama3.2
```

**Alternative models** (if llama3.2 doesn't work):
```bash
# Smaller, faster (recommended for testing)
ollama pull llama3.2:1b

# Medium size, good balance
ollama pull llama3.2

# Larger, better quality (slower)
ollama pull llama3.1
```

**Download time:** 5-15 minutes depending on your internet speed

---

## ✅ Step 3: Verify Ollama is Running

### Check if Ollama service is running:
```bash
ollama list
```

You should see your downloaded models listed.

### Test the model:
```bash
ollama run llama3.2
```

Type a test message like "Hello!" and press Enter. If you get a response, it's working!

Type `/bye` to exit the chat.

---

## 🔧 Step 4: Configure SalesAI

Your `.env` file is already configured:
```env
OLLAMA_MODEL=llama3.2
OLLAMA_URL=http://localhost:11434
OLLAMA_TIMEOUT_S=120
```

**Note:** I changed the timeout from 30 to 120 seconds to prevent timeout errors.

---

## 🚀 Step 5: Run SalesAI

### Option A: Web Interface (Recommended)
```bash
# Terminal 1: Start Ollama (if not already running)
ollama serve

# Terminal 2: Start SalesAI backend
python api.py

# Then open frontend/index.html in your browser
```

### Option B: Command Line
```bash
# Make sure Ollama is running
ollama serve

# In another terminal
python main.py
```

---

## 🐛 Troubleshooting

### Error: "Cannot connect to Ollama server"

**Solution 1:** Start Ollama service
```bash
ollama serve
```

**Solution 2:** Check if Ollama is running
- Open Task Manager (Ctrl+Shift+Esc)
- Look for "ollama" process
- If not running, start it: `ollama serve`

---

### Error: "Ollama request timed out"

**Cause:** Model is taking too long to respond (common with large models)

**Solution 1:** Use a smaller/faster model
```bash
ollama pull llama3.2:1b
```
Then update `.env`:
```env
OLLAMA_MODEL=llama3.2:1b
```

**Solution 2:** Increase timeout in `.env`
```env
OLLAMA_TIMEOUT_S=300
```

**Solution 3:** Make sure your computer has enough RAM
- llama3.2:1b needs ~2GB RAM
- llama3.2 needs ~4GB RAM
- llama3.1 needs ~8GB RAM

---

### Error: "Model not found"

**Solution:** Pull the model first
```bash
ollama pull llama3.2
```

---

### Ollama is slow / taking forever

**Causes:**
- Large model on slow hardware
- Not enough RAM
- CPU-only inference (no GPU)

**Solutions:**
1. **Use smaller model:**
   ```bash
   ollama pull llama3.2:1b
   ```

2. **Close other applications** to free up RAM

3. **Check system requirements:**
   - Minimum: 8GB RAM
   - Recommended: 16GB RAM
   - GPU: Optional but much faster

---

## 📊 Model Comparison

| Model | Size | RAM Needed | Speed | Quality |
|-------|------|------------|-------|---------|
| llama3.2:1b | ~1GB | 2GB | ⚡⚡⚡ Fast | ⭐⭐ Good |
| llama3.2 | ~2GB | 4GB | ⚡⚡ Medium | ⭐⭐⭐ Great |
| llama3.1 | ~4GB | 8GB | ⚡ Slow | ⭐⭐⭐⭐ Excellent |

**Recommendation:** Start with `llama3.2` for best balance.

---

## 🔍 Verify Everything Works

Run this test script:

```bash
python test_ollama.py
```

This will:
1. Check if Ollama is running
2. Test text generation
3. Test JSON generation
4. Show response times

---

## 💡 Tips for Better Performance

### 1. Keep Ollama Running
Don't stop/start Ollama between requests. Keep it running:
```bash
ollama serve
```

### 2. Warm Up the Model
First request is always slower. Run a test first:
```bash
ollama run llama3.2 "test"
```

### 3. Use Appropriate Model Size
- **Testing/Development:** llama3.2:1b (fastest)
- **Production:** llama3.2 (balanced)
- **Best Quality:** llama3.1 (slowest)

### 4. Monitor Resource Usage
- Open Task Manager
- Watch RAM and CPU usage
- If maxed out, use smaller model

---

## 🎯 Quick Commands Reference

```bash
# Install/Update Ollama
# Download from: https://ollama.com/download

# Start Ollama server
ollama serve

# List installed models
ollama list

# Download a model
ollama pull llama3.2

# Test a model
ollama run llama3.2

# Remove a model (to save space)
ollama rm llama3.1

# Check Ollama version
ollama --version

# Get help
ollama --help
```

---

## 🆘 Still Having Issues?

1. **Restart Ollama:**
   ```bash
   # Stop Ollama (close terminal or Ctrl+C)
   # Then start again:
   ollama serve
   ```

2. **Restart Computer:**
   Sometimes Windows needs a restart after installing Ollama

3. **Check Ollama Logs:**
   - Windows: `%LOCALAPPDATA%\Ollama\logs`
   - Look for error messages

4. **Reinstall Ollama:**
   - Uninstall from Windows Settings
   - Download fresh installer
   - Install again

---

## ✅ Success Checklist

- [ ] Ollama installed
- [ ] Model downloaded (`ollama pull llama3.2`)
- [ ] Ollama running (`ollama serve`)
- [ ] Model tested (`ollama run llama3.2`)
- [ ] `.env` configured
- [ ] SalesAI backend starts without errors
- [ ] Frontend can generate emails

---

## 🚀 Ready to Go!

Once everything is checked, run:
```bash
python api.py
```

Then open `frontend/index.html` and enjoy your AI-powered sales emails! 🎉
