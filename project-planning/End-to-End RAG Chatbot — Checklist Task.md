# RAG Chatbot — Progress Checklist

> Track your progress through the end-to-end RAG chatbot project.

---

## Phase 0 — Core Concepts

> No code — vocabulary only. Read and understand each concept before moving on.

- [x] **Concept 0.1** — What is an LLM?
- [x] **Concept 0.2** — What are tokens?
- [x] **Concept 0.3** — What is a context window?
- [x] **Concept 0.4** — What are chunks and text splitting?
- [x] **Concept 0.5** — What are embeddings?
- [x] **Concept 0.6** — What is a vector store / vector database?
- [x] **Concept 0.7** — What is a prompt template?

---

## Phase 1 — Foundations

> Goal: Get a document Q&A working in a terminal script — no API, no frontend yet.

### Step 1 · What is RAG?
- [x] Read: [LangChain — RAG Conceptual Guide](https://python.langchain.com/docs/concepts/rag/)
- [x] Read: [OpenAI Cookbook — Question Answering with RAG](https://cookbook.openai.com/examples/question_answering_using_embeddings)
- [x] Build: Python script that loads a PDF, chunks it, embeds with OpenAI, stores in FAISS/Chroma
- [x] Build: Script takes a terminal question and returns an answer

### Step 2 · Move embeddings to PostgreSQL with pgvector
- [x] Read: [pgvector README](https://github.com/pgvector/pgvector)
- [x] Read: [LangChain — PGVector integration](https://python.langchain.com/docs/integrations/vectorstores/pgvector/)
- [x] Build: Replace in-memory vector store with PostgreSQL + pgvector

---

## Phase 2 — Build the API

> Goal: Expose your RAG logic as HTTP endpoints.

### Step 3 · FastAPI app with `/upload` endpoint
- [x] Read: [FastAPI — Getting Started](https://fastapi.tiangolo.com/tutorial/)
- [ ] Read: [LangChain — Document Loaders (PDF)](https://python.langchain.com/docs/how_to/document_loader_pdf/)
- [x] Build: `POST /upload` — accepts PDF, chunks it, saves embeddings to pgvector

### Step 4 · Add the `/chat` endpoint
- [ ] Read: [LangChain — LCEL Introduction](https://python.langchain.com/docs/concepts/lcel/)
- [ ] Read: [LangChain — RAG Chains](https://python.langchain.com/docs/tutorials/rag/)
- [ ] Read: [OpenAI API — Chat Completions](https://platform.openai.com/docs/guides/text-generation)
- [x] Build: `POST /chat` — retrieves relevant chunks, sends to LLM, returns answer

---

## Phase 3 — Streaming

> Goal: Return responses token by token like ChatGPT.

### Step 5 · Stream the LLM response
- [ ] Read: [FastAPI — StreamingResponse](https://fastapi.tiangolo.com/advanced/custom-response/#streamingresponse)
- [ ] Read: [LangChain — Streaming](https://python.langchain.com/docs/how_to/streaming/)
- [ ] Build: Modify `/chat` to stream tokens using `StreamingResponse`
- [ ] Test: Verify streaming with `curl` or a simple HTML page with `fetch` + `ReadableStream`

---

## Phase 4 — Chat History

> Goal: Save conversations to Postgres, reload old chats, give the LLM memory.

### Step 6 · Save chat history to PostgreSQL
- [ ] Read: [LangChain — Chat History](https://python.langchain.com/docs/concepts/chat_history/)
- [ ] Read: [LangChain — How to add message history](https://python.langchain.com/docs/how_to/message_history/)
- [ ] Build: `conversations` table and `messages` table in Postgres
- [ ] Build: `POST /conversations` and `GET /conversations/{id}/messages` endpoints
- [ ] Build: `/chat` now saves both user message and AI response to DB

### Step 7 · Reconstruct conversation context
- [ ] Read: [LangChain — RunnableWithMessageHistory](https://python.langchain.com/docs/how_to/message_history/)
- [ ] Build: Load previous messages from DB and include in every new LLM call

---

## Phase 5 — LangGraph

> Goal: Replace manual chain with a proper agent graph.

### Step 8 · LangGraph fundamentals
- [ ] Read: [LangGraph — Quickstart](https://langchain-ai.github.io/langgraph/tutorials/introduction/)
- [ ] Read: [LangGraph — Concepts: Nodes, Edges, State](https://langchain-ai.github.io/langgraph/concepts/)
- [ ] Build: Rewrite RAG logic as a LangGraph graph with nodes: `retrieve_docs → format_context → generate_answer → save_message`

### Step 9 · LangGraph checkpointing
- [ ] Read: [LangGraph — Persistence / Checkpointing](https://langchain-ai.github.io/langgraph/concepts/persistence/)
- [ ] Read: [LangGraph — How to add persistence](https://langchain-ai.github.io/langgraph/how-tos/persistence/)
- [ ] Build: Add Postgres checkpointer — graph state saved automatically at every node

---

## Phase 6 — Frontend

> Goal: Chat interface with streaming display and drag & drop PDF upload.

### Step 10 · Chat UI with streaming
- [ ] Read: [MDN — Fetch API & ReadableStream](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API/Using_readable_streams)
- [ ] Read: [Vercel AI SDK](https://sdk.vercel.ai/docs)
- [ ] Build: React chat page with input + message list
- [ ] Build: Tokens stream in progressively as the LLM responds

### Step 11 · Drag & drop PDF upload
- [ ] Read: [MDN — HTML Drag and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)
- [ ] Read: [react-dropzone docs](https://react-dropzone.js.org/)
- [ ] Build: Drag PDF into chat area → `POST /upload` → confirmation message in chat

---

## Phase 7 — Production Readiness

> Goal: Portfolio-ready project.

### Step 12 · Auth + multi-user sessions
- [ ] Read: [FastAPI — Security / OAuth2 with JWT](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/)
- [ ] Build: JWT auth — each user has isolated documents and chat history

### Step 13 · Prompt versioning
- [ ] Read: [OpenAI — Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [ ] Build: Move system prompts to config/DB with version tracking — nothing hardcoded

### Step 14 · Deployment
- [ ] Read: [FastAPI — Deployment with Docker](https://fastapi.tiangolo.com/deployment/docker/)
- [ ] Setup: [Supabase](https://supabase.com/) — managed Postgres + pgvector, free tier
- [ ] Build: Dockerize the app
- [ ] Deploy: App running on a public URL

---

## Progress Summary

| Phase | Steps | Status |
|---|---|---|
| 0 · Core Concepts | 7 concepts | |
| 1 · Foundations | Steps 1–2 | |
| 2 · API | Steps 3–4 | |
| 3 · Streaming | Step 5 | |
| 4 · Chat History | Steps 6–7 | |
| 5 · LangGraph | Steps 8–9 | |
| 6 · Frontend | Steps 10–11 | |
| 7 · Production | Steps 12–14 | |