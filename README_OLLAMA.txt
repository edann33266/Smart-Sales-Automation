╔══════════════════════════════════════════════════════════════╗
║              SalesAI with Ollama - Quick Guide               ║
╚══════════════════════════════════════════════════════════════╝

✅ WHAT YOU HAVE NOW:
  • Beautiful modern frontend (fixed send buttons!)
  • Working backend with Ollama support
  • Email preamble text removed
  • Better error messages
  • Increased timeout (120s) to prevent timeouts

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚀 SETUP STEPS (First Time):

1. INSTALL OLLAMA
   → Download: https://ollama.com/download/windows
   → Run installer
   → Ollama starts automatically

2. DOWNLOAD AI MODEL
   → Open Command Prompt
   → Run: ollama pull llama3.2
   → Wait 5-10 minutes (downloads ~2GB)

3. TEST OLLAMA
   → Run: python test_ollama.py
   → Should see all tests pass ✅

4. START SALESAI
   → Terminal 1: ollama serve
   → Terminal 2: python api.py
   → Browser: Open frontend/index.html

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 DAILY USE (After Setup):

Terminal 1:
  $ ollama serve
  (Keep running)

Terminal 2:
  $ python api.py
  (Keep running)

Browser:
  Open: frontend/index.html
  1. Select a lead
  2. Click "Generate Email"
  3. Wait 30-60 seconds
  4. Review email
  5. Click "Send Email"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️ COMMON ISSUES & FIXES:

Problem: "Cannot connect to Ollama server"
Fix: Make sure "ollama serve" is running

Problem: "Model not found"
Fix: Run "ollama pull llama3.2"

Problem: "Request timed out"
Fix 1: Use faster model "ollama pull llama3.2:1b"
Fix 2: Increase timeout in .env: OLLAMA_TIMEOUT_S=300

Problem: Ollama is very slow
Fix: Use smaller model "llama3.2:1b" (faster but less quality)

Problem: Frontend shows "Backend Offline"
Fix: Make sure "python api.py" is running

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 MODEL COMPARISON:

llama3.2:1b  → ⚡⚡⚡ Super Fast  | ⭐⭐ Good      | 2GB RAM
llama3.2     → ⚡⚡ Medium      | ⭐⭐⭐ Great    | 4GB RAM
llama3.1     → ⚡ Slow         | ⭐⭐⭐⭐ Best   | 8GB RAM

Recommended: llama3.2 (best balance)

To switch models:
  1. Download: ollama pull llama3.2:1b
  2. Edit .env: OLLAMA_MODEL=llama3.2:1b
  3. Restart backend

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎨 FRONTEND IMPROVEMENTS:

✅ Two send buttons (header + main) - both work!
✅ Beautiful gradient design
✅ Live backend status indicator
✅ Toast notifications with icons
✅ Loading spinners during generation
✅ Color-coded lead scores
✅ Smooth animations
✅ Selected lead highlighting

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔧 BACKEND IMPROVEMENTS:

✅ Removed email preamble text
✅ Better error messages
✅ Increased timeout (30s → 120s)
✅ Cleaner email output
✅ Better JSON parsing

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📚 HELPFUL FILES:

START_WITH_OLLAMA.md  → Quick start guide
OLLAMA_SETUP.md       → Detailed setup instructions
test_ollama.py        → Test if Ollama is working
ARCHITECTURE.md       → How everything works
UI_FEATURES.md        → Frontend features guide

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🆘 NEED HELP?

1. Run test: python test_ollama.py
2. Read: OLLAMA_SETUP.md
3. Check: .env configuration

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ QUICK CHECKLIST:

□ Ollama installed
□ Model downloaded (ollama pull llama3.2)
□ Ollama running (ollama serve)
□ Backend running (python api.py)
□ Frontend opened (frontend/index.html)
□ Can select leads
□ Can generate emails
□ Can send emails

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎉 YOU'RE ALL SET!

Your SalesAI is now configured to use Ollama (local AI).
No API keys needed, everything runs on your computer!

Enjoy generating AI-powered sales emails! 🚀

╔══════════════════════════════════════════════════════════════╗
║  Questions? Check OLLAMA_SETUP.md for detailed help         ║
╚══════════════════════════════════════════════════════════════╝
