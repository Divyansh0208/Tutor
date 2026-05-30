<div align="center">

# 🎓 Sage — AI Tutor

**A real-time AI tutoring chatbot powered by Groq and Meta's LLaMA 3.3-70B.**  
Fast. Structured. Beautiful. Built for learners.

<br/>

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-3.0+-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![Groq](https://img.shields.io/badge/Groq-LLaMA%203.3--70B-FF6600?style=for-the-badge&logo=meta&logoColor=white)](https://groq.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](LICENSE)

</div>

---

## Preview

![Sage UI](screenshot.png)

> The full app lives on a single page — sidebar for sessions on the left, chat area on the right, and mode controls (Precise · Balanced · Creative) pinned to the top-right corner.

---

## What is Sage?

Sage is a zero-database, single-file AI tutoring app. Ask it anything — recursion, calculus, history, chemistry — and it responds with a structured breakdown: an analogy, step-by-step explanation, a concrete example, and a follow-up question to solidify your understanding.

It's powered by [Groq's LPU inference](https://groq.com/) for near-instant replies from Meta's **LLaMA 3.3-70B** model. No accounts, no cloud storage — clone it, add your API key, and start learning in under two minutes.

---

## UI Overview

The entire app is a single page with three zones:

| Zone | What it does |
|---|---|
| **Left sidebar** | Lists all chat sessions. Create, switch, rename, or delete them. Persisted in `localStorage` — survives page reloads. |
| **Chat area** | Main conversation view. Bot messages render full Markdown — bold, italic, inline code, fenced code blocks with copy buttons, and lists. |
| **Top-right controls** | Three mode pills — **Precise**, **Balanced**, **Creative** — plus a live **Online** status indicator for the Groq connection. |
| **Bottom input bar** | Labelled *"Ask Sage anything…"* — type your question and hit **Send**. |

---

## Features

| | Feature | Details |
|---|---|---|
| 🧠 | **Structured Tutoring** | Every answer: analogy → steps → example → follow-up question |
| 💬 | **Multi-Session Chats** | Create, rename, switch, delete sessions — all in `localStorage` |
| 🎯 | **Three Response Modes** | Precise (0.2) · Balanced (0.5) · Creative (1.3) temperature presets |
| ⚡ | **Groq-Powered Speed** | Near-instant inference via Groq's dedicated LPU hardware |
| 📝 | **Rich Markdown Rendering** | Code blocks with copy buttons, bold, italic, lists, inline code |
| 🔒 | **Conversation Context** | Sends the last 20 messages for coherent multi-turn sessions |
| 📱 | **Responsive Design** | Collapsible sidebar — works on desktop and mobile |

---

## Quick Start

### Prerequisites

- **Python 3.10+**
- A free **Groq API key** — get one at [console.groq.com](https://console.groq.com/)

### 1. Clone the repo

```bash
git clone https://github.com/Divyansh0208/Tut.git
cd "AI tutor"
```

### 2. Create and activate a virtual environment

```bash
python -m venv env

# Windows
env\Scripts\activate

# macOS / Linux
source env/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add your API key

Create a `.env` file in the project root:

```env
GROQ_API_KEY="gsk_your_api_key_here"
```

> [!WARNING]
> Never commit your `.env` file. It is already listed in `.gitignore`.

### 5. Run the app

```bash
python app.py
```

Open **http://127.0.0.1:5000** — the app loads instantly, no build step required.

---

## Project Structure

```
AI tutor/
├── app.py               # Flask backend + entire HTML/CSS/JS frontend (one file)
├── requirements.txt     # Python dependencies
├── .env                 # Groq API key — never commit this
├── .gitignore           # Ignores .env, env/, __pycache__/
├── env/                 # Virtual environment (gitignored)
└── README.md            # You are here
```

The full frontend — HTML, CSS, and JavaScript — is embedded inside `app.py` as a template string. There is no build tool, no bundler, and no separate static directory.

---

## How It Works

```
Browser
├── Sidebar  →  session list, stored in localStorage
└── Chat UI  →  user bubbles + Markdown-rendered bot replies
         │
         │  POST /api/tutor  (fetch)
         ▼
Flask (app.py)
├── GET  /          → serves the single-page app
└── POST /api/tutor → validates input, injects system prompt, calls Groq
         │
         ▼
Groq Cloud API
└── LLaMA 3.3-70B  →  structured tutoring response
```

State is entirely client-side. No database, no server sessions — just `localStorage`.

---

## API Reference

### `POST /api/tutor`

**Request** (`application/json`):

```json
{
  "message": "Explain recursion in Python",
  "history": [
    { "role": "user",      "content": "What is a function?" },
    { "role": "assistant", "content": "A function is a reusable block of code..." }
  ],
  "mode": "balanced",
  "temperature": 0.5
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `message` | `string` | ✅ | The learner's question |
| `history` | `array` | ❌ | Prior turns for context — last 20 used |
| `mode` | `string` | ❌ | `"precise"` · `"balanced"` · `"creative"` |
| `temperature` | `float` | ❌ | Manual override `0.0 – 2.0` |

**Success** (`200`):
```json
{ "reply": "Think of recursion like a set of Russian nesting dolls..." }
```

**Errors**:

| Code | Cause |
|---|---|
| `400` | Message is empty or missing |
| `500` | Groq API error |
| `503` | `GROQ_API_KEY` not set |

---

## Response Modes

Switchable from the top-right pill controls in the UI:

| Mode | Temp | Best For |
|---|---|---|
| 🎯 **Precise** | `0.2` | Math, code, factual Q&A |
| ⚖️ **Balanced** | `0.5` | General tutoring — the default |
| 🎨 **Creative** | `1.3` | Analogies, brainstorming, open-ended questions |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python · Flask |
| Frontend | Vanilla HTML · CSS · JavaScript |
| AI Model | Meta LLaMA 3.3-70B-Versatile |
| Inference | Groq Cloud API (LPU) |
| Fonts | Inter · JetBrains Mono (Google Fonts) |

---

## Contributing

1. Fork the repo and create a branch: `git checkout -b feature/your-idea`
2. Make your changes and test locally with `python app.py`
3. Open a pull request with a clear description of the change

One feature or fix per PR keeps reviews fast.

---

## License

Released under the [MIT License](LICENSE).

---

<div align="center">

Built with 💜 by [Divyansh](https://github.com/Divyansh0208)

</div>
