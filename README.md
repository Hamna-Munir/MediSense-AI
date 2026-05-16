# 🩺 MediSense AI

> **Intelligent Health Triage for Underserved Communities**

[![Hackathon](https://img.shields.io/badge/Gemma%204%20Good%20Hackathon-2026-ff4d8d?style=flat-square)](https://kaggle.com/competitions/gemma-4-good-hackathon)
[![Track](https://img.shields.io/badge/Track-Health%20%26%20Sciences-ff7043?style=flat-square)](#)
[![Model](https://img.shields.io/badge/Model-LLaMA%203.3%2070B-orange?style=flat-square)](https://console.groq.com)
[![Powered by](https://img.shields.io/badge/Inference-Groq-blueviolet?style=flat-square)](https://groq.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](#license)

---

<p align="center">
  <strong>4.5 billion people lack access to basic primary healthcare.</strong><br/>
  MediSense AI puts a structured clinical triage assistant in the hands of every community health worker — in under 90 seconds, on any device with a browser.
</p>

---

## 📌 Table of Contents

- [The Problem](#-the-problem)
- [What It Does](#-what-it-does)
- [Demo](#-demo)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [How the Triage Works](#-how-the-triage-works)
- [Architecture](#-architecture)
- [Prompt Engineering](#-prompt-engineering)
- [Ethical Considerations](#-ethical-considerations)
- [Roadmap](#-roadmap)
- [License](#-license)

---

## 🌍 The Problem

The WHO estimates a global shortfall of **15 million health workers**, concentrated almost entirely in low- and middle-income countries. In rural Pakistan, sub-Saharan Africa, and remote Southeast Asia:

- The nearest clinic may be **3–6 hours away**
- Community Health Workers (CHWs) with minimal training are often the **only** first responder
- Misclassifying an emergency as non-urgent — or vice versa — **can be fatal**

A CHW standing in front of a patient with chest pain and shortness of breath needs to know: *Is this urgent? Should this person travel three hours to a hospital right now, or rest at home?*

---

## ✨ What It Does

MediSense AI is a **single HTML file** that delivers AI-powered health triage in under 90 seconds.

| Feature | Description |
|---|---|
| 🏷️ **Symptom Tags** | 14 quick-tap symptom buttons — no typing required |
| 📝 **Free Text Input** | Open field for detailed symptom description |
| 📊 **Severity Slider** | Color-coded 1–10 scale (green → amber → red) |
| 🤖 **AI Triage** | LLaMA 3.3 70B assigns urgency: LOW / MODERATE / HIGH / EMERGENCY |
| 📋 **Clinical Output** | Summary, possible conditions, next steps, red flags |
| 🚨 **Emergency Alerts** | Full-screen banner for life-threatening cases |
| 💬 **Follow-up Chat** | Conversational Q&A with full context memory |
| 📱 **Zero Install** | Works on any browser — share via WhatsApp, email, USB |

---

## 🎬 Demo

> **Live Demo:** [your-netlify-url.netlify.app](#)  
> **Demo Video:** [YouTube →](#)

**Five test cases covered in the notebook:**

| Case | Input | Expected Output |
|---|---|---|
| Mild Cold | Runny nose, sore throat, 2 days | 🟢 LOW |
| Persistent Fever | 38.8°C, 4 days, hypertension | 🟡 MODERATE |
| Abdominal Pain | Lower right, 8hrs, severity 8 | 🟠 HIGH |
| Chest Pain | Radiating, sweating, 62M | 🔴 EMERGENCY |
| Pediatric Rash | Child, blotchy rash, mild fever | 🟡 MODERATE |

---

## 🛠 Tech Stack

| Layer | Technology | Reason |
|---|---|---|
| **Frontend** | Vanilla HTML / CSS / JS | Zero dependencies — deployable anywhere |
| **LLM** | LLaMA 3.3 70B | Best open model for medical reasoning |
| **Inference** | Groq API | ~750 tokens/sec, sub-3s latency |
| **Output format** | Strict JSON schema | Typed, parseable, always consistent |
| **Fonts** | Playfair Display + Plus Jakarta Sans | Legible on low-res screens |
| **Notebook** | Jupyter / Kaggle | Reproducible demo + evaluation |

---

## 📁 Project Structure

```
medisense-ai/
│
├── health-triage-ai.html       # Main application (single file, deploy anywhere)
├── medisense_ai.ipynb          # Kaggle notebook — demo, evaluation, analysis
├── medisense-ai-writeup.md     # Hackathon writeup (Kaggle submission format)
└── README.md                   # You are here
```

---

## 🚀 Getting Started

### Option 1 — Run the HTML App (Recommended)

1. Open `health-triage-ai.html` in any browser
2. Inside the file, find this line and replace with your Groq key:
   ```js
   const GROQ_API_KEY = 'YOUR_GROQ_API_KEY_HERE';
   ```
3. Save and open — no server needed

Get a free Groq API key at [console.groq.com](https://console.groq.com) — keys start with `gsk_`.

### Option 2 — Run the Jupyter Notebook

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/medisense-ai.git
cd medisense-ai

# Install dependency
pip install groq

# Set your API key
export GROQ_API_KEY=gsk_your_key_here

# Launch notebook
jupyter notebook medisense_ai.ipynb
```

### Option 3 — Kaggle

1. Upload `medisense_ai.ipynb` to a new Kaggle notebook
2. Add your key under **Add-ons → Secrets → GROQ_API_KEY**
3. Enable **Internet** in notebook settings
4. Click **Run All**

### Option 4 — Deploy to Netlify

1. Rename `health-triage-ai.html` → `index.html`
2. Drag the file onto [app.netlify.com](https://app.netlify.com) deploy dropzone
3. Done — your site is live instantly

---

## 🔬 How the Triage Works

MediSense collects structured patient data and sends it to the LLM as a clinical prompt. The model responds with a strict JSON object:

```json
{
  "triage": "HIGH",
  "triageLabel": "Seek Immediate Care",
  "summary": "19-year-old male presenting with severe lower right abdominal pain...",
  "conditions": ["Appendicitis", "Ovarian cyst", "Mesenteric adenitis"],
  "steps": "• Go to nearest emergency clinic immediately\n• Do not eat or drink\n• Keep patient still and comfortable",
  "redFlags": "• Pain spreading across abdomen\n• Fever above 38.5°C\n• Rigid abdomen",
  "urgent": ""
}
```

The four triage levels:

| Level | Label | Action |
|---|---|---|
| 🟢 **LOW** | Non-Urgent / Monitor at Home | Rest, monitor, self-care |
| 🟡 **MODERATE** | See a Doctor Today | Clinic visit within 24 hours |
| 🟠 **HIGH** | Seek Immediate Care | Hospital visit today — do not delay |
| 🔴 **EMERGENCY** | Call Emergency Services Now | Life-threatening — act immediately |

---

## 🏗 Architecture

```
User (Browser)
│
├── Patient Intake (Age / Sex / History)
├── Symptom Tags (14 quick-tap options)
├── Free Text Description
├── Duration + Severity Slider
│
└──► Structured Clinical Prompt
          │
          ▼
   Groq API — llama-3.3-70b-versatile
   (~750 tokens/sec · <3s latency)
          │
          ▼
   JSON Response Parser
          │
     ┌────┴──────────────────────┐
     ▼                           ▼
Triage Result Card         Follow-up Chat
• Urgency level            • Full context kept
• Clinical summary         • Multi-turn Q&A
• Possible conditions      • Empathetic tone
• Next steps
• Red flags
• Emergency banner
```

---

## 🧠 Prompt Engineering

Three key decisions that make the output reliable:

**1. Strict JSON output**
The system prompt instructs the model to return only a JSON object — no markdown, no preamble. This means the output is always machine-readable without any regex cleanup.

**2. Conservative escalation bias**
The prompt explicitly says *"when in doubt, always escalate to the higher level."* In clinical triage, a false negative (missed emergency) is far worse than a false positive (unnecessary urgency).

**3. Explicit triage definitions**
Each level (LOW / MODERATE / HIGH / EMERGENCY) is defined with precise clinical language in the prompt, eliminating ambiguity between adjacent levels.

---

## ⚖️ Ethical Considerations

**MediSense is not a medical device.** It does not replace clinical judgment and must not be used as the sole basis for treatment decisions.

| Risk | Mitigation |
|---|---|
| LLM hallucination | Conservative bias; always surface red flags; show reasoning |
| Over-reliance | Persistent "Preliminary AI Analysis" label on every result |
| Training data bias | Outputs framed as "possible conditions" not "confirmed diagnosis" |
| Data privacy | Fully stateless — zero patient data stored anywhere |
| Language barrier | English-only currently; multilingual support planned |

---

## 🗺 Roadmap

- [x] Core triage engine (4 urgency levels)
- [x] Follow-up conversational chat
- [x] Single-file HTML deployment
- [x] Kaggle notebook with evaluation
- [ ] Multilingual UI — Urdu, Swahili, Hindi
- [ ] Offline mode — Gemma 4 E2B/E4B via LiteRT or llama.cpp
- [ ] Bluetooth vital signs — pulse oximeter, thermometer integration
- [ ] Auto-generated referral letter for clinic visits
- [ ] CHW training module embedded in app
- [ ] Population-level analytics dashboard

---

## 📄 License

MIT License — free to use, modify, and distribute. See [LICENSE](LICENSE) for details.

---

<p align="center">
  Built for the <strong>Gemma 4 Good Hackathon 2026</strong> · Health & Sciences Track<br/>
  Powered by <strong>Groq + LLaMA 3.3 70B</strong><br/><br/>
  <em>"When the right tools are accessible to everyone, the possibilities for positive change are truly endless."</em>
</p>
