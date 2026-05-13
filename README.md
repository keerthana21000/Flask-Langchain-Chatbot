# Flask + LangChain Chatbot

A conversational AI chatbot built with Flask and LangChain, powered by Groq's ultra-fast LLM inference. The app maintains per-session chat history so the assistant remembers the context of your conversation.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Web Framework | [Flask](https://flask.palletsprojects.com/) |
| LLM Orchestration | [LangChain](https://www.langchain.com/) |
| LLM Provider | [Groq](https://groq.com/) (`llama-3.3-70b-versatile`) |
| Frontend | Vanilla HTML, CSS, JavaScript |

---

## Screenshot

> _Screenshot placeholder — add a screenshot of the chat UI here._

---

## Setup Instructions

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd chatbot
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Add your Groq API key

Open the `.env` file and replace the placeholder with your actual key:

```
GROQ_API_KEY=your-groq-api-key-here
```

You can get a free API key at [console.groq.com](https://console.groq.com).

### 4. Run the app

```bash
python app.py
```

Then open your browser and go to **http://127.0.0.1:5000**.

---

## How It Works

```
User types a message
        │
        ▼
  Flask POST /chat
        │
        ▼
  LangChain Pipeline
  ┌─────────────────────────────────┐
  │  ChatPromptTemplate             │
  │  (system prompt + history +     │
  │   user input)                   │
  │            │                    │
  │            ▼                    │
  │  RunnableWithMessageHistory     │
  │  (injects past messages from    │
  │   in-memory session store)      │
  │            │                    │
  │            ▼                    │
  │  ChatGroq LLM                   │
  │  (llama-3.3-70b-versatile)      │
  └─────────────────────────────────┘
        │
        ▼
  Response content extracted
        │
        ▼
  JSON { "reply": "..." }
  returned to the browser
        │
        ▼
  JavaScript appends bot message
  to the chat window
```

1. **User input** — the user types a message and hits Send or Enter.
2. **Flask** — the `/chat` route receives the message as JSON via a `POST` request.
3. **LangChain prompt** — the message is inserted into a `ChatPromptTemplate` alongside a system instruction and the session's message history.
4. **Memory** — `RunnableWithMessageHistory` looks up the session's `ChatMessageHistory` from an in-memory store and injects prior messages so the model has full context.
5. **Groq** — the composed prompt is sent to Groq's API, which runs the `llama-3.3-70b-versatile` model and returns a response at high speed.
6. **Reply** — the response content is extracted and returned to the browser as `{ "reply": "..." }`, where JavaScript renders it in the chat window.

---

## Project Structure

```
chatbot/
├── app.py               # Flask app, LangChain chain, routes
├── requirements.txt     # Python dependencies
├── .env                 # Groq API key (not committed)
├── .gitignore
└── templates/
    └── index.html       # Chat UI (HTML + CSS + JS)
```
