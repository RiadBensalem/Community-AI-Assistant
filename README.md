# 🤖 AI Onboarding Assistant for Communities

This project implements a **stateful AI agent** designed to assist in managing and scaling community onboarding. The agent can answer questions using **RAG (Retrieval-Augmented Generation)**, **evaluate new applications** using LLM-based scoring, and provide an interactive chat interface via **Gradio** — all built and tested in a **Kaggle notebook environment**.

I started this project after completing a 5 days intensive GenAI course: [Course link](https://blog.google/technology/developers/google-kaggle-genai-intensive/)

![Global Architecture](./ai-assistant.png)
---

## 🧠 What This Agent Does

- ✅ **Answers Community Questions**  
  Uses a vector database (ChromaDB) and semantic search to retrieve relevant information from internal documentation (PDFs), powered by Gemini embeddings and LangChain’s RAG integration.

- 📝 **Scores New Member Applications**  
  Takes answers from a Google Form stored in a Google Sheet, and evaluates each application based on predefined criteria using Gemini LLM scoring with structured output (JSON schema).

- 🔍 **Tool-Using Agent Architecture**  
  Built using [LangGraph](https://docs.langgraph.dev/) to define the agent as a state machine with tools and routing logic.

- 💬 **Gradio Chat Interface**  
  Provides a styled, interactive chat experience where users can ask questions, invoke tools implicitly, and view agent logs and scoring results in real-time.

---

## 🧱 Technologies Used

| Component | Description |
|----------|-------------|
| **LangGraph** | Defines the AI agent as a graph of nodes (chat, tool handling, scoring). |
| **LangChain + Gemini** | LLM interface for chat and scoring tasks (via `ChatGoogleGenerativeAI`). |
| **ChromaDB** | Vector store to enable fast semantic search for RAG. |
| **Python requests** | Pulls new applications submitted through a Google Form by reading a public spreadsheet |
| **Gradio** | Provides a fully interactive frontend in the notebook. |
| **PyMuPDF & `unstructured`** | Used for chunking and extracting content from community PDF files. |

---

## 🔄 How It Works

### 1. RAG (Retrieval-Augmented Generation)

- PDFs about the community (mission, goals, values, policies) are processed into semantic chunks using three strategies:
  - Fixed-size + overlap
  - One chunk per document
  - Semantic chunking via `unstructured`
- Chunks are embedded using Gemini’s `text-embedding-004` model and stored in ChromaDB.
- `get_info(query: str)` is a tool that performs semantic search and returns a formatted markdown snippet.

### 2. Application Scoring

- The `score_application()` and `check_new_applications()` tools allow the agent to:
  - Evaluate individual or batch applications.
  - Parse responses into a JSON schema with score, verdict (`approve`, `review`, `reject`), and detailed reasoning.
  - Save the evaluations to a CSV and display results in a styled table.

### 3. Agent Logic with LangGraph

- Nodes:
  - `chatbot`: LLM chat interaction
  - `score_node`: Handles structured scoring
  - `tools`: Automatically resolves tool calls
  - `human`: Manages user input from Gradio

- Routing:
  - Routes messages from chatbot to tools if tool calls are present
  - Routes back to chat node after tool execution
  - Ends when user types “q” or “quit”

---

## 💻 Running in Kaggle Notebook

All components (agent logic, UI, RAG setup, and evaluation) are implemented in a Kaggle Notebook. The notebook includes:

- Inline markdown explanations
- Tool-calling logic
- Cell-level breakdown for easy learning
- Integration with Google Sheets via API key

You can duplicate the notebook and run everything end-to-end without extra setup.

---

## 🧪 Example Prompts

- “What is the mission of this community?”
- “Can you check and score new applications?”
- “How can I contribute as a new member?”
- “Can you score this application: ...”

---

## 📦 Folder Structure

```
.
├── ai-agent-community-assistant.ipynb   # Main development notebook
├── community-docs/                          # Directory with PDF docs
├── scored_applications.csv                  # Output file with scored apps
└── README.md                                # This file
```

---

## ✅ Results

- Built and deployed a fully functional GenAI agent from scratch.
- Solidified understanding of agents, embeddings, RAG, and prompt engineering.
- Learned LangGraph by implementing custom nodes and conditional routing.
- Wrapped everything into an interactive, usable UI using Gradio.

---

## 📚 Background

This project was created as a follow-up to the [Google x Kaggle Generative AI Intensive (Q1 2025)](https://rsvp.withgoogle.com/events/google-generative-ai-intensive_2025q1/home) — a 5-day, hands-on course on LLMs, embeddings, agent tools, and MLOps.

---

## ✨ Try It or Extend It

Fork this project or run the notebook on Kaggle to:
- Add more tools or community documents
- Score applicants using different criteria
- Extend the UI with buttons, file uploads, or user feedback

---

## 🙌 Credits

Built by [Riad Bensalem] — feel free to reach out or connect on [LinkedIn](https://www.linkedin.com/in/riad-bensalem-429799127/).
