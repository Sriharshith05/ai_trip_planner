# AI Trip Planner

An agentic AI travel planner that generates complete, real-time trip itineraries — day-by-day plans, hotel and restaurant suggestions, weather forecasts, local transportation options, and full cost breakdowns — for any destination worldwide.

Built with **LangGraph** (ReAct agent), **FastAPI** (backend), and **Streamlit** (chat UI), the agent uses live tools to pull real-time data instead of relying on the LLM's static knowledge.

---

## ✨ Features

- 🗺️ Day-by-day itinerary generation, with both a classic tourist plan and an off-the-beaten-path alternative
- 🌦️ Live weather lookups (current + forecast) via OpenWeatherMap
- 📍 Real-time place search (attractions, restaurants, activities, transportation) via Google Places, with Tavily as an automatic fallback
- 💰 Automatic cost breakdown and per-day budget calculation
- 💱 Live currency conversion
- 💬 Simple chat interface via Streamlit

---

## 🧱 Tech Stack

| Layer               | Tool                                                                 |
| ------------------- | -------------------------------------------------------------------- |
| Agent orchestration | [LangGraph](https://langchain-ai.github.io/langgraph/) (ReAct agent) |
| LLM                 | [Groq](https://console.groq.com/) (default) or OpenAI                |
| Backend API         | FastAPI + Uvicorn                                                    |
| Frontend            | Streamlit                                                            |
| Place data          | Google Places API (with Tavily Search fallback)                      |
| Weather data        | OpenWeatherMap API                                                   |
| Currency data       | ExchangeRate-API                                                     |

---

## 📁 Project Structure

```
ai_trip_planner/
├── main.py                     # FastAPI backend – exposes POST /query
├── streamlit_app.py            # Streamlit chat frontend
├── config/
│   └── config.yaml             # LLM provider + model name config
├── agent/
│   └── agent_workflow.py       # LangGraph ReAct agent definition
├── tools/                      # LangChain tool wrappers (weather, places, expenses, currency)
├── utils/                      # Underlying service clients + helpers
├── prompt_library/
│   └── prompt.py                # System prompt for the agent
├── requirements.txt
├── pyproject.toml
└── .env                          # Your API keys (not committed — see below)
```

---

## ⚙️ Setup

### 1. Clone and enter the project

```bash
git clone https://github.com/Sriharshith05/ai_trip_planner.git
cd ai_trip_planner
```

### 2. Create and activate a virtual environment

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS/Linux
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure environment variables

Copy the template and fill in your keys:

```bash
cp .env.name .env      # Windows: copy .env.name .env
```

Edit `.env` with the following **exact variable names**:

```dotenv
GROQ_API_KEY=""              # required — get one free at console.groq.com
GPLACES_API_KEY=""           # required for Google Places search
TAVILY_API_KEY=""            # required — used as fallback search
OPENWEATHERMAP_API_KEY=""    # required for weather lookups
EXCHANGE_RATE_API_KEY=""     # required for currency conversion
OPENAI_API_KEY=""            # optional — only needed if you switch provider to "openai"
```

> **Note:** `GOOGLE_API_KEY` and `FOURSQUARE_API_KEY` from the original template are unused by the codebase and can be left blank or removed.

### 5. Check the configured LLM model

`config/config.yaml` controls which model is used. Groq periodically deprecates models — if you get a `model_not_found` error, check [Groq's current model list](https://console.groq.com/docs/models) and update:

```yaml
llm:
  groq:
    provider: "groq"
    model_name: "openai/gpt-oss-120b" # update if deprecated
```

---

## ▶️ Running the App

You need **two terminals**, both with the virtual environment activated:

**Terminal 1 — start the backend**

```bash
uvicorn main:app --reload --port 8000
```

**Terminal 2 — start the frontend**

```bash
streamlit run streamlit_app.py
```

Open the URL Streamlit prints (typically `http://localhost:8501`) and try a query like:

> _"Plan a trip to Goa for 5 days"_

---

## 🔑 Getting API Keys

| Key                      | Where to get it                                                                 |
| ------------------------ | ------------------------------------------------------------------------------- |
| `GROQ_API_KEY`           | [console.groq.com](https://console.groq.com/) — free tier available             |
| `GPLACES_API_KEY`        | [Google Cloud Console](https://console.cloud.google.com/) — enable "Places API" |
| `TAVILY_API_KEY`         | [tavily.com](https://tavily.com/) — free tier available                         |
| `OPENWEATHERMAP_API_KEY` | [openweathermap.org/api](https://openweathermap.org/api) — free tier available  |
| `EXCHANGE_RATE_API_KEY`  | [exchangerate-api.com](https://www.exchangerate-api.com/) — free tier available |

---

## ⚠️ Disclaimer

This tool generates AI-assisted travel plans. Always verify prices, operating hours, visa requirements, and other details independently before your trip.

---
