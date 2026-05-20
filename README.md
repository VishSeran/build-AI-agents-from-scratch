# 🤖 Building AI Agents from Scratch with Python

A hands-on project that teaches you how to design, build, and orchestrate **multi-agent AI systems** from scratch using Python. Two complete real-world applications are built end-to-end: a **Weather AI Agent** and **The Daily Dish** — a restaurant customer-service chatbot powered by Retrieval-Augmented Generation (RAG).

---

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Agent Type 1 — Weather AI Agent](#agent-type-1--weather-ai-agent)
- [Agent Type 2 — The Daily Dish Chatbot](#agent-type-2--the-daily-dish-chatbot)
- [Tech Stack](#tech-stack)
- [Setup & Installation](#setup--installation)
- [Usage](#usage)
- [Key Concepts](#key-concepts)
- [Learning Objectives](#learning-objectives)

---

## Overview

This project demonstrates how to build **production-style AI agents** without relying on high-level agent frameworks. Every component — retrieval, memory, query understanding, and LLM-based response generation — is implemented from first principles in Python.

By the end, you will have built two fully functional multi-agent systems:

| Project | Agents | Key Techniques |
|---|---|---|
| Weather AI Agent | 3 agents | Live API integration, in-memory state |
| The Daily Dish Chatbot | 4 agents | RAG, TF-IDF retrieval, PDF parsing, LLM generation |

---

## Project Structure

```
.
├── Building_AI_Agents_from_Scratch_-_Daily_Dish-v1.ipynb   # Main notebook
├── The_Daily_Dish_FAQ.pdf                                   # FAQ knowledge base for the chatbot
└── README.md
```

---

## Agent Type 1 — Weather AI Agent

### What it does

The Weather AI Agent answers natural-language weather questions by fetching live data from the OpenWeatherMap API, persisting context in memory, and generating friendly human-readable responses.

### Architecture

```
User (Weather Question)
        │
        ▼
┌──────────────────────┐
│  Weather Retrieval   │  Agent 1 — Calls OpenWeatherMap API,
│  Agent               │  returns temperature, humidity, conditions
└────────┬─────────────┘
         │
         ▼
┌──────────────────────┐
│  Memory Agent        │  Agent 2 — Stores past queries and
│                      │  weather responses for context recall
└────────┬─────────────┘
         │
         ▼
┌──────────────────────┐
│  Response Agent      │  Agent 3 — Combines live data + memory
│  (LLM-based)         │  to produce a natural-language reply
└────────┬─────────────┘
         │
         ▼
    User (Final Reply)
```

### Agents

| Agent | Responsibility |
|---|---|
| `WeatherRetrievalAgent` | Hits the OpenWeatherMap REST API and returns structured weather data |
| `MemoryAgent` | Stores and recalls previous city queries across the conversation |
| `ResponseAgent` | Formats the data into a clear, user-friendly sentence |

### Prerequisites

- OpenWeatherMap API key → [Get one here](https://home.openweathermap.org/api_keys)

---

## Agent Type 2 — The Daily Dish Chatbot

### What it does

A RAG-powered customer-service chatbot for a fictional restaurant, *The Daily Dish*. It reads a PDF of FAQs, retrieves the most relevant passages using TF-IDF cosine similarity, maintains conversation memory, and generates answers via an LLM.

### Architecture

```
User (Question)
        │
        ▼
┌──────────────────────────┐
│  Query Understanding     │  Agent 1 — Interprets user intent,
│  Agent                   │  extracts key terms and keywords
└────────┬─────────────────┘
         │
         ▼
┌──────────────────────────┐
│  Document Retrieval      │  Agent 2 — Searches the FAQ PDF using
│  Agent                   │  TF-IDF vectorisation + cosine similarity
└────────┬─────────────────┘
         │
         ▼
┌──────────────────────────┐
│  Memory Agent            │  Agent 3 — Persists conversation history
│                          │  to support contextual, personalised replies
└────────┬─────────────────┘
         │
         ▼
┌──────────────────────────┐
│  LLM Response Agent      │  Agent 4 — Combines retrieved FAQ context
│  (OpenAI / GPT)          │  + memory and generates a friendly answer
└────────┬─────────────────┘
         │
         ▼
    User (Final Reply)
```

### Agents

| Agent | Responsibility |
|---|---|
| `QueryUnderstandingAgent` | Parses and interprets the user's question to extract intent |
| `DocumentRetrievalAgent` | Chunks the FAQ PDF and retrieves the top-k relevant passages via TF-IDF |
| `MemoryAgent` | Maintains a rolling conversation history |
| `ResponseAgent` (LLM) | Synthesises retrieved content and memory into a coherent answer |

### Knowledge Base

The chatbot's knowledge comes from `The_Daily_Dish_FAQ.pdf` — a restaurant FAQ document covering reservations, menu items, hours, and more. You can swap this PDF for any domain-specific document.

---

## Tech Stack

| Library | Purpose |
|---|---|
| `requests` | OpenWeatherMap API calls |
| `PyPDF2` / `pypdf` | PDF text extraction |
| `scikit-learn` | TF-IDF vectorisation and cosine similarity |
| `nltk` | Text tokenisation and preprocessing |
| `numpy` | Vector operations |
| `python-dotenv` | Environment variable management |
| OpenAI API | LLM-powered response generation |

---

## Setup & Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/building-ai-agents-from-scratch.git
cd building-ai-agents-from-scratch
```

### 2. Install dependencies

```bash
pip install requests python-dotenv pypdf scikit-learn nltk
```

### 3. Configure API keys

Create a `.env` file in the project root:

```env
WEATHER_API_KEY=your_openweathermap_api_key
OPENAI_API_KEY=your_openai_api_key
```

Or set them directly in the notebook before running.

### 4. Download the FAQ PDF

The Daily Dish FAQ is available at:
```
https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/7vgNfis17dQfjHAiIKkBOg/The-Daily-Dish-FAQ.pdf
```

Place it in the project root as `The_Daily_Dish_FAQ.pdf`.

### 5. Launch the notebook

```bash
jupyter notebook "Building_AI_Agents_from_Scratch_-_Daily_Dish-v1.ipynb"
```

> ⚠️ **Note:** Restart the kernel after each `pip install` step as instructed in the notebook.

---

## Usage

### Weather Agent

```python
# Initialize agents
weather_agent = WeatherRetrievalAgent(WEATHER_API_KEY)
memory_agent  = MemoryAgent()
response_agent = ResponseAgent()

# Run a query
city = "London"
weather_info = weather_agent.get_weather(city)
memory_agent.store(city, weather_info)
response = response_agent.generate_response(city, weather_info)

print(response)
# → "The current weather in London is light rain with a temperature of 14°C and humidity of 78%."
```

### Daily Dish Chatbot

```python
# Load, chunk, and index the FAQ PDF, then query the multi-agent pipeline
question = "Do you take reservations for large groups?"
answer = chatbot_pipeline(question)
print(answer)
```

---

## Key Concepts

- **Multi-Agent Systems (MAS):** Breaking a complex task into specialised, composable agents rather than one monolithic model.
- **Retrieval-Augmented Generation (RAG):** Grounding LLM responses in external documents to reduce hallucination and keep answers factual.
- **Agent Memory:** Persisting conversation state so agents can deliver contextual, personalised replies across multiple turns.
- **TF-IDF Retrieval:** A classic, lightweight approach to semantic document search without requiring embeddings or a vector database.
- **Prompt Engineering:** Structuring prompts that combine retrieved context and memory to guide LLM output effectively.

---

