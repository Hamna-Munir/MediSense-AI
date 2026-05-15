# MediSense AI — Intelligent Health Triage for Underserved Communities

### Bridging the global primary care gap with AI-powered symptom triage, offline-first design, and conversational follow-up — built for the 4.5 billion people without reliable access to a doctor.

---

## The Problem

More than **4.5 billion people** worldwide lack access to basic primary healthcare. In rural Pakistan, sub-Saharan Africa, and remote corners of Southeast Asia, the nearest clinic may be hours away. Community health workers (CHWs) — often minimally trained — are the first and sometimes only point of contact for patients experiencing potentially serious symptoms.

The challenge is not a lack of caring people. It is a lack of **clinical decision support** at the moment it is needed most. A CHW standing in front of a patient with chest pain and shortness of breath needs to know: *is this urgent? Should this person travel three hours to a hospital right now, or rest and monitor at home?*

Getting that wrong can be fatal.

---

## The Solution: MediSense AI

**MediSense AI** is a lightweight, browser-based health triage assistant that combines structured symptom intake with a large language model to deliver rapid, explainable preliminary guidance — categorized into four actionable urgency levels.

It is built as a **single HTML file** — no server, no installation, no app store. It runs on any device with a browser, including low-end Android phones common in underserved regions.

---

## Track

**Impact Track — Health & Sciences**

> *Bridge the gap between humans and data. Build tools that accelerate discovery or democratize knowledge.*

---

## How It Works

### 1. Patient Intake
The user (CHW or patient) enters basic demographic information:
- Age
- Biological sex
- Pre-existing medical conditions (free text)

### 2. Symptom Collection (Dual Input)
MediSense uses a **hybrid input system** designed for users with limited literacy or typing ability:

- **Quick-tap symptom tags** — 14 pre-defined common symptoms (Fever, Chest Pain, Shortness of Breath, etc.) that can be selected with a single tap
- **Free-text description** — an open field for nuanced symptom description in the patient's own words

This dual approach maximizes accessibility. A CHW can complete an intake in under 60 seconds.

### 3. Severity & Duration
- A **10-dot color-coded severity slider** (green → amber → red) captures pain intensity without requiring numerical literacy
- A duration dropdown captures symptom timeline (from "less than 1 hour" to "more than 2 weeks")

### 4. AI Triage via Groq + LLaMA 3.3 70B
All collected data is compiled into a structured clinical prompt and sent to **Groq's inference API** running `llama-3.3-70b-versatile`. The model is instructed to return a strict JSON payload containing:

```json
{
  "triage": "LOW | MODERATE | HIGH | EMERGENCY",
  "triageLabel": "Human-readable urgency label",
  "summary": "2-3 sentence clinical summary",
  "conditions": ["Possible condition 1", "Possible condition 2", "..."],
  "steps": "Bullet-point recommended next steps",
  "redFlags": "Bullet-point warning signs to escalate",
  "urgent": "Emergency action message if applicable"
}
```

The model response is parsed and rendered into a structured results card with color-coded triage indicators.

### 5. Conversational Follow-Up
After the initial assessment, users can ask natural-language follow-up questions ("Should I avoid certain foods?", "What does this mean for my child?") via a persistent chat interface. Full conversation history is maintained for context-aware responses.

---

## Architecture

```
User (Browser)
     │
     ▼
HTML/CSS/JS (Single File — No Dependencies)
     │
     ├── Symptom Tag Selection  ──┐
     ├── Free Text Input          ├──► Structured Clinical Prompt
     ├── Age / Sex / History      │
     ├── Duration + Severity  ────┘
     │
     ▼
Groq API  (llama-3.3-70b-versatile)
     │
     ▼
JSON Response Parser
     │
     ▼
Rendered Triage Card
     ├── Urgency Level (LOW / MODERATE / HIGH / EMERGENCY)
     ├── Clinical Summary
     ├── Possible Conditions
     ├── Next Steps
     ├── Red Flags
     └── Emergency Banner (if applicable)
     │
     ▼
Follow-Up Chat (Stateful, context-aware)
```

**Key technical choices:**

| Component | Choice | Rationale |
|---|---|---|
| Runtime | Vanilla HTML/CSS/JS | Zero dependencies, runs offline-capable, no build step |
| LLM | LLaMA 3.3 70B via Groq | Best-in-class open model, sub-second inference, free tier |
| Output format | Strict JSON prompt | Ensures parseable, structured, consistent triage output |
| Fonts | Google Fonts (Playfair Display + Plus Jakarta Sans) | Elegant, legible on small screens |
| Deployment | Single `.html` file | Works on USB stick, WhatsApp share, email attachment |

---

## Triage System

MediSense uses a four-level triage system modeled on international emergency medicine frameworks (START triage, Manchester Triage System):

| Level | Label | Color | Meaning |
|---|---|---|---|
| LOW | Non-Urgent / Monitor at Home | 🟢 Green | Symptoms manageable without immediate care |
| MODERATE | See a Doctor Today | 🟡 Amber | Professional evaluation needed within 24 hours |
| HIGH | Seek Immediate Care | 🟠 Orange | Go to a clinic or hospital now |
| EMERGENCY | Call Emergency Services Now | 🔴 Red | Life-threatening — act immediately |

The LLM is prompted with medical context and constrained to select one of these four levels, with an accompanying human-readable label and optional urgent action message for EMERGENCY cases.

---

## Design Philosophy

MediSense is built around three design principles for global health equity:

**1. Accessibility over aesthetics**
The interface uses large tap targets, emoji-augmented labels, and color-coded severity selection to work for users with limited literacy or digital experience. No login, no onboarding, no friction.

**2. Trust through transparency**
Every result includes a clearly labeled "Preliminary AI Analysis" tag and a persistent disclaimer that MediSense is not a diagnostic tool. The output shows *why* a level was assigned — clinical summary, red flags, possible conditions — rather than just a number. Explainability builds trust.

**3. Deployable anywhere**
A single `.html` file can be shared via WhatsApp, saved to a USB drive, emailed to a clinic, or hosted on a $1/month server. No app stores, no operating system requirements, no data plan dependency beyond the Groq API call itself.

---

## Real-World Impact

**Target users:**
- Community health workers in rural and underserved regions
- Patients in areas with limited clinic access
- School health nurses and first responders without clinical training
- Remote telemedicine support operators

**Potential reach:**
According to the WHO, there is a global shortfall of **15 million health workers**, concentrated in low- and middle-income countries. Tools that extend the capabilities of non-specialist CHWs can meaningfully reduce preventable deaths from misclassified emergencies — particularly for time-sensitive conditions like sepsis, stroke, myocardial infarction, and anaphylaxis.

A CHW using MediSense spends **under 90 seconds** completing an intake and receives structured clinical guidance, red flags to monitor, and a clear recommendation on urgency — guidance that previously required a trained clinician or a multi-hour journey to a hospital.

---

## Challenges & How We Solved Them

**Challenge 1: Reliable structured output from LLM**
Unconstrained LLM output is unpredictable. We solved this by using a strict system prompt requiring JSON-only responses with a defined schema, combined with client-side JSON parsing with error handling.

**Challenge 2: Accessibility for low-literacy users**
Free-text-only input excludes users who struggle to describe symptoms in writing. The quick-tap symptom tag system allows full symptom capture with zero typing, while the free-text field remains available for detail.

**Challenge 3: Preventing over-reliance**
Medical AI tools risk being treated as diagnostic authorities. We addressed this through persistent disclaimers, transparent labeling of AI outputs, and prompting the LLM to always frame outputs as preliminary guidance rather than diagnoses.

**Challenge 4: Deployment constraints**
Many target environments have unreliable internet and no server infrastructure. The single-file architecture means MediSense can be saved locally and used without any server — only the Groq API call requires connectivity, and that call takes under 3 seconds.

---

## Limitations & Ethical Considerations

- **MediSense is not a medical device.** It does not replace clinical judgment and must not be used as a sole basis for treatment decisions.
- **LLM hallucination risk.** The model may suggest plausible but incorrect conditions. Red flags and urgency levels are the most safety-critical outputs and should always be validated against local clinical protocols.
- **Bias in training data.** LLaMA 3.3 was trained predominantly on English-language medical literature. Symptom presentations that differ by population, region, or endemic disease profile may be underrepresented.
- **No persistent patient data.** MediSense stores nothing. Each session is stateless. This is a privacy feature but prevents longitudinal tracking.

**Future mitigations:** Fine-tuning on locally-relevant clinical guidelines (e.g., WHO Essential Medicines, regional endemic disease profiles), adding multilingual support for Urdu, Swahili, and Hindi, and integrating with offline-capable edge models for zero-connectivity environments.

---

## Future Roadmap

- **Multilingual support** — Urdu, Swahili, Hindi, Arabic using LLM translation layer
- **Offline mode** — Integrate a quantized edge model (Gemma 4 E2B/E4B) via LiteRT or llama.cpp for full offline operation in zero-connectivity environments
- **Vital signs integration** — Connect to low-cost Bluetooth pulse oximeters and thermometers for objective data
- **CHW training module** — Add an embedded education layer so health workers can learn from each case
- **Referral letter generation** — Auto-generate a structured referral summary the patient can carry to the clinic

---

## Links

- **Live Demo:** *(attach HTML file or host URL)*
- **Code Repository:** *(attach GitHub link)*
- **Demo Video:** *(attach YouTube link)*

---

## Conclusion

MediSense AI demonstrates that a single HTML file, a carefully crafted prompt, and a fast open-source LLM can meaningfully extend the reach of healthcare into the places that need it most. It is not a replacement for doctors — it is a force multiplier for the community health workers, nurses, and patients who cannot access one.

When the right tools are accessible to everyone, the possibilities for positive change are truly endless.

---

*Built for the Gemma 4 Good Hackathon 2026 · Health & Sciences Track · Powered by Groq + LLaMA 3.3 70B*
