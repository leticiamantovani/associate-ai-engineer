# End-to-End RAG Chatbot — Learning Roadmap

> **Goal:** Build a full production-ready RAG chatbot where users upload PDFs, ask questions about them, see responses streamed in real time, and have chat history saved and reloaded from a database.

---

## 🗺️ Project Overview

**Stack:** Python · FastAPI · LangChain/LangGraph · PostgreSQL · pgvector · OpenAI API · React

**Features to build (as recommended by Matheus):**
- ✅ Streaming responses
- ✅ Observable pattern to save messages on node end (LangGraph)
- ✅ Frontend with streaming
- ✅ Reconstruct chat from what's saved in the database
- ✅ Drag & drop in the chat area sends documents to vector DB, chunked in real time
- ✅ RAG over those documents to answer the user

---

## Phase 0 — Core Concepts You Must Know First

> **Goal:** Before writing a single line of code, understand the vocabulary. Every term in this phase will appear constantly throughout the project.

---

### Concept 0.1 · What is an LLM?

**What it is:** A Large Language Model (LLM) is an AI model trained on massive amounts of text that can understand and generate human language. Examples: GPT-4, Claude, Gemini. When you call the OpenAI API in this project, you are sending text to an LLM and receiving text back.

**The key mental model:** An LLM is a function. You give it text (called a **prompt**), it gives you text back (called a **completion** or **response**). That's it at the surface level.

**Read:**
- [OpenAI — How LLMs work (plain English)](https://platform.openai.com/docs/introduction)
- [Illustrated intro to LLMs by Jay Alammar](https://jalammar.github.io/illustrated-gpt2/) ← visual, highly recommended

---

### Concept 0.2 · What are Tokens?

**What it is:** LLMs don't read text character by character or word by word — they read **tokens**. A token is roughly a word fragment. For example, "chatbot" = 1 token, "extraordinarily" = 4 tokens. Spaces, punctuation, and common word endings are all separate tokens.

**Why it matters for this project:**
1. **Streaming** works by sending one token at a time as the LLM generates them — that's why you see text appearing word by word in ChatGPT.
2. **Context window** (explained next) is measured in tokens, not characters.
3. You pay for OpenAI API calls per token.

**Read:**
- [OpenAI — Tokenizer (interactive)](https://platform.openai.com/tokenizer) ← paste any text and see how it's split into tokens

---

### Concept 0.3 · What is a Context Window?

**What it is:** An LLM can only "see" a limited amount of text at once — this is its **context window**, measured in tokens. GPT-4o, for example, has a 128k token context window. Everything you want the LLM to know (your question, the document content, the conversation history) must fit inside this window.

**Why it matters for this project:**
- A PDF might be 50 pages long — that's way too much to dump into the context window at once.
- This is exactly **why RAG exists**: instead of sending the whole document, you send only the relevant pieces. More on this in Step 1.
- Chat history also has this problem: long conversations eventually don't fit. You'll handle this in Phase 4.

---

### Concept 0.4 · What are Chunks and Text Splitting?

**What it is:** Since a full document won't fit in a context window, you break it into smaller pieces called **chunks**. **Text splitting** is the process of dividing a document into chunks of a target size (e.g., 500 tokens each), usually with some overlap between consecutive chunks so context isn't lost at the boundaries.

**Example:**
```
Original PDF (10,000 tokens)
↓ split into chunks of 500 tokens with 50-token overlap
→ [chunk_1: tokens 1–500]
→ [chunk_2: tokens 451–950]   ← 50-token overlap with chunk_1
→ [chunk_3: tokens 901–1400]  ← 50-token overlap with chunk_2
→ ...
```

**Why overlap?** If a sentence starts at token 490 and ends at token 510, without overlap it would be cut in half between chunk_1 and chunk_2 and lose meaning. Overlap ensures continuity.

**Read:**
- [LangChain — Text Splitters](https://python.langchain.com/docs/concepts/text_splitters/)

---

### Concept 0.5 · What are Embeddings?

**What it is:** An **embedding** is a list of numbers (a vector) that represents the *meaning* of a piece of text. Similar texts produce similar vectors. This is how you can search by meaning, not just by keyword.

**Example:**
```
"How do I cancel my subscription?"       → [0.23, -0.87, 0.41, ...]  (similar vectors)
"Steps to unsubscribe from the service"  → [0.25, -0.85, 0.39, ...]  (similar meaning!)
"The weather is nice today"              → [-0.91, 0.12, -0.55, ...] (completely different)
```

**Why it matters:** When a user asks a question, you convert their question into an embedding and then search the database for chunks with similar embeddings. That's how you find the *relevant* parts of the document — not by matching keywords, but by matching meaning.

**Read:**
- [OpenAI — Embeddings Guide](https://platform.openai.com/docs/guides/embeddings)
- [Visual explanation of embeddings by Simon Willison](https://simonwillison.net/2023/Oct/23/embeddings/) ← great visual article

---

### Concept 0.6 · What is a Vector Store / Vector Database?

**What it is:** A **vector store** is a database optimized for storing and searching embeddings (vectors). Instead of `SELECT * WHERE name = 'foo'`, you query it with "find me the 5 vectors most similar to this one." This is called a **similarity search** or **nearest neighbor search**.

**In this project you'll use two options:**
- `FAISS` or `Chroma` — in-memory, no setup, great for early steps
- `pgvector` — a PostgreSQL extension that adds vector search to a regular Postgres DB, which is what you'll use in production

**The mental model:**
```
User question
  → convert to embedding
  → search vector store
  → get top 3 most similar chunks
  → send those chunks to LLM as context
```

**Read:**
- [LangChain — Vector Stores overview](https://python.langchain.com/docs/concepts/vectorstores/)

---

### Concept 0.7 · What is a Prompt Template?

**What it is:** A **prompt template** is a reusable string with variables that gets filled in at runtime before being sent to the LLM. Instead of hardcoding your prompt, you define a template and inject the user's question and the retrieved document chunks into it.

**Example:**
```
You are a helpful assistant. Use the following context to answer the question.

Context:
{retrieved_chunks}

Question: {user_question}

Answer:
```

This template has two variables: `{retrieved_chunks}` (filled with the relevant PDF pieces) and `{user_question}` (filled with what the user typed).

**Read:**
- [LangChain — Prompt Templates](https://python.langchain.com/docs/concepts/prompt_templates/)

---

## Phase 1 — Foundations (Weeks 1–2)

> **Goal:** Understand what RAG is and get a document Q&A working in a script — no API, no frontend yet.

---

### Step 1 · What is RAG?

**Concept:** Now that you know what chunks, embeddings, and vector stores are, RAG makes sense: **Retrieval-Augmented Generation** is a pattern where instead of asking an LLM a question directly, you first *retrieve* the relevant chunks of a document, then *augment* the LLM prompt with those chunks, then *generate* the answer.

**The full flow:**
```
1. User asks: "What are the payment terms in this contract?"
2. Convert question to embedding
3. Search vector store → find top 3 most relevant chunks from the contract PDF
4. Build a prompt: "Given this context: [chunk1] [chunk2] [chunk3] — answer: What are the payment terms?"
5. Send prompt to LLM → get answer grounded in the actual document
```

**Why not just send the whole document?** Because it won't fit in the context window (Concept 0.3), and even if it did, it's expensive and slower.

**Read:**
- [LangChain — RAG Conceptual Guide](https://python.langchain.com/docs/concepts/rag/)
- [OpenAI Cookbook — Question Answering with RAG](https://cookbook.openai.com/examples/question_answering_using_embeddings)

**Build:**
A Python script that:
1. Loads a hardcoded PDF file
2. Splits it into chunks (using LangChain's text splitter)
3. Embeds the chunks with OpenAI
4. Stores them in memory (use `FAISS` or `Chroma` locally — no DB yet)
5. Takes a question from the terminal and returns an answer

```
rag_script.py → "What does this PDF say about X?" → answer printed in terminal
```

---

### Step 2 · What is pgvector, and why does Postgres need an extension for vectors?

**Concept:** Regular Postgres stores text, numbers, dates — but it has no native concept of "find me rows that are *semantically similar* to this." `pgvector` is an extension that adds a new column type (`vector`) and new search operators for similarity search (using cosine distance, L2 distance, etc.) directly inside Postgres.

**Why this is great:** Instead of running a separate vector database (like Pinecone or Weaviate), you can keep everything — your user data, chat history, AND your document embeddings — in one Postgres database. This simplifies your architecture significantly.

**Read:**
- [pgvector README](https://github.com/pgvector/pgvector)
- [LangChain — PGVector integration](https://python.langchain.com/docs/integrations/vectorstores/pgvector/)

**Build:**
Replace the in-memory vector store from Step 1 with PostgreSQL + pgvector. Your script now persists chunks between runs.

```
Upload PDF → chunks saved to Postgres with pgvector → query retrieves relevant chunks
```

---

## Phase 2 — Build the API (Weeks 3–4)

> **Goal:** Expose your RAG logic as HTTP endpoints. This is where your backend skills kick in.

---

### Step 3 · What is a Document Loader? And how does FastAPI compare to NestJS?

**Concept — Document Loader:** A Document Loader is a LangChain abstraction that reads a file (PDF, Word doc, web page, etc.) and returns it as a list of `Document` objects — each one containing the text content plus metadata (filename, page number, etc.). It handles the messy work of parsing different file formats so your code doesn't have to.

**Concept — FastAPI vs NestJS:** FastAPI is Python's equivalent of NestJS. Both are modern, decorator-based frameworks for building REST APIs. Key differences:
- FastAPI uses Python type hints for automatic request validation (similar to NestJS DTOs, but less boilerplate)
- FastAPI auto-generates interactive Swagger docs at `/docs` out of the box with zero config
- No dependency injection container like NestJS — simpler, more explicit
- Performance is comparable for I/O-bound work (which this project is)

**Read:**
- [FastAPI — Getting Started](https://fastapi.tiangolo.com/tutorial/)
- [LangChain — Document Loaders (PDF)](https://python.langchain.com/docs/how_to/document_loader_pdf/)

**Build:**

```
POST /upload → accepts a PDF file → chunks it → saves embeddings to pgvector
```

---

### Step 4 · What is a Chain in LangChain?

**Concept:** A **chain** in LangChain is a sequence of steps piped together. Output of step A becomes input to step B. The most common chain for RAG is:

```
retrieve relevant chunks → format into a prompt → send to LLM → return answer
```

LangChain uses a syntax called **LCEL** (LangChain Expression Language) with a `|` pipe operator — similar to Unix pipes or RxJS operators if you've seen those.

**Example:**
```python
chain = retriever | prompt_template | llm | output_parser
result = chain.invoke({"question": "What are the payment terms?"})
```

**Read:**
- [LangChain — LCEL Introduction](https://python.langchain.com/docs/concepts/lcel/)
- [LangChain — RAG Chains](https://python.langchain.com/docs/tutorials/rag/)
- [OpenAI API — Chat Completions](https://platform.openai.com/docs/guides/text-generation)

**Build:**

```
POST /chat { "message": "What does the PDF say about X?" }
→ retrieves relevant chunks → sends to OpenAI → returns answer
```

> ✅ At this point, you have a working RAG API.

---

## Phase 3 — Streaming (Week 5)

> **Goal:** Return responses token by token like ChatGPT, instead of waiting for the full answer.

---

### Step 5 · What is SSE (Server-Sent Events) and how does streaming work end to end?

**Concept — How LLM streaming works:** LLMs generate tokens one at a time. Without streaming, your API waits until the LLM finishes generating all tokens, then returns everything at once (could be 5–15 seconds for a long answer). With streaming, you return each token to the client as soon as it's generated — so the user sees text appearing progressively, just like ChatGPT.

**Concept — SSE (Server-Sent Events):** SSE is an HTTP feature where the server keeps a connection open and pushes data chunks to the client over time. The client reads them as they arrive using a `ReadableStream`. It's simpler than WebSockets for one-way server-to-client streaming, which is exactly what you need here.

**The full flow:**
```
Client sends POST /chat
→ Server opens streaming connection (keeps it open)
→ LLM generates token "The"      → server sends it immediately → client displays "The"
→ LLM generates token " payment" → server sends it            → client displays "The payment"
→ LLM generates token " terms"   → server sends it            → client displays "The payment terms"
→ LLM finishes → server closes connection
```

**Read:**
- [FastAPI — StreamingResponse](https://fastapi.tiangolo.com/advanced/custom-response/#streamingresponse)
- [LangChain — Streaming](https://python.langchain.com/docs/how_to/streaming/)

**Build:**
Modify your `/chat` endpoint to stream tokens back using `StreamingResponse`. Test it with `curl` or a simple HTML page with `fetch` + `ReadableStream`.

```
POST /chat → tokens arrive one by one instead of all at once
```

---

## Phase 4 — Chat History (Weeks 6–7)

> **Goal:** Save conversations to Postgres, reload old chats, and give the LLM memory of the conversation.

---

### Step 6 · What is chat history in the context of LLMs, and what is the context window limit problem?

**Concept — LLMs are stateless:** Every API call is completely independent. If you ask "What is the capital of France?" and the LLM answers "Paris", then you ask "What's the population there?", the LLM has no idea what "there" refers to — unless you re-send the previous messages every time.

**Chat history** is the list of previous messages you include in each API call so the LLM has context:

```json
[
  { "role": "user",      "content": "What is the capital of France?" },
  { "role": "assistant", "content": "The capital of France is Paris." },
  { "role": "user",      "content": "What's the population there?" }
]
```

**The context window limit problem:** The longer the conversation, the more tokens you send with each request. Eventually it won't fit in the context window. Common solutions: summarize old messages, or keep only the last N turns. You don't need to solve this perfectly now — just be aware it exists and is a real production concern.

**Read:**
- [LangChain — Chat History](https://python.langchain.com/docs/concepts/chat_history/)
- [LangChain — How to add message history](https://python.langchain.com/docs/how_to/message_history/)

**Build:**
- A `conversations` table and a `messages` table in Postgres
- New endpoints: `POST /conversations`, `GET /conversations/{id}/messages`
- Your `/chat` endpoint now saves both the user message and the AI response

---

### Step 7 · Reconstruct conversation context from the database

**Concept:** When a user reopens an old conversation, you load all previous messages from Postgres and include them in the next API call. The LLM doesn't store anything — you are the memory, and you pass it back each time.

This is what Matheus meant by "reconstruct chat from what's saved in the DB."

**Read:**
- [LangChain — RunnableWithMessageHistory](https://python.langchain.com/docs/how_to/message_history/)

**Build:**

```
User opens old conversation → load messages from DB → next LLM call includes full history
```

---

## Phase 5 — LangGraph (Weeks 8–9)

> **Goal:** Replace your manual chain with a proper agent graph — the *observable pattern to save messages on node end* that Matheus mentioned.

---

### Step 8 · What is the difference between LangChain and LangGraph? What is a graph?

**Concept — LangChain vs LangGraph:**
- **LangChain** is the toolkit: document loaders, prompt templates, chains, vector store integrations. It gives you building blocks.
- **LangGraph** is built on top of LangChain and adds **stateful, cyclical graphs** — meaning your LLM pipeline can loop, branch, and maintain shared state across steps. It gives you the architecture to assemble those building blocks into a production application.

**Concept — What is a graph (nodes & edges)?**
A graph is a way to represent steps and their connections:
- **Nodes** = individual steps (functions) in your pipeline
- **Edges** = connections between steps — "after this node finishes, go to that node"
- **State** = a shared object that gets passed between nodes and updated at each step

**Example graph for your chatbot:**
```
START
  ↓
[retrieve_docs]    ← searches pgvector for relevant chunks
  ↓
[format_context]   ← builds the prompt with chunks + chat history
  ↓
[generate_answer]  ← calls the LLM
  ↓
[save_message]     ← persists the answer to Postgres
  ↓
END
```

**Concept — Observable pattern on node end:** Each node in LangGraph fires an event when it completes. You can attach listeners to these events to save data, log, or emit metrics — without polluting your business logic. This is the "observable pattern" Matheus mentioned.

**Read:**
- [LangGraph — Quickstart](https://langchain-ai.github.io/langgraph/tutorials/introduction/)
- [LangGraph — Concepts: Nodes, Edges, State](https://langchain-ai.github.io/langgraph/concepts/)

**Build:**
Rewrite your RAG logic as a LangGraph graph with explicit nodes:

```
[retrieve_docs] → [format_context] → [generate_answer] → [save_message]
```

---

### Step 9 · What is checkpointing in LangGraph?

**Concept:** **Checkpointing** means saving the full state of your graph at every node transition, automatically. LangGraph has a built-in Postgres checkpointer — after each node runs, it writes the current state to the DB. This gives you:

1. **Fault tolerance:** If something fails mid-graph, you can resume from where it left off
2. **Observability:** You can inspect exactly what happened at each step
3. **Automatic chat history persistence:** You don't need your manual message-saving code from Step 6 anymore — LangGraph handles it through checkpointing

This is the production-grade, clean version of what you built manually in Phase 4.

**Read:**
- [LangGraph — Persistence / Checkpointing](https://langchain-ai.github.io/langgraph/concepts/persistence/)
- [LangGraph — How to add persistence](https://langchain-ai.github.io/langgraph/how-tos/persistence/)

**Build:**
Use LangGraph's Postgres checkpointer to automatically save graph state at every node.

---

## Phase 6 — Frontend (Weeks 10–11)

> **Goal:** Build the UI — chat interface with streaming display and drag & drop PDF upload.

---

### Step 10 · What is ReadableStream and how do you consume SSE in React?

**Concept:** When your FastAPI backend streams tokens, the browser receives them as a `ReadableStream` — a JavaScript object representing data that arrives over time. You read it chunk by chunk using a reader, updating the UI as each token arrives.

**The pattern in plain JavaScript:**
```javascript
const response = await fetch('/chat', { method: 'POST', body: ... });
const reader = response.body.getReader();

while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  const token = new TextDecoder().decode(value);
  // append token to the displayed message in state
}
```

The Vercel AI SDK wraps all of this in clean React hooks so you don't have to write the reader loop yourself.

**Read:**
- [MDN — Fetch API & ReadableStream](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API/Using_readable_streams)
- [Vercel AI SDK](https://sdk.vercel.ai/docs) ← recommended, handles streaming abstractions for React

**Build:**
A React page with:
- A chat input + message list
- Tokens streaming in as the LLM responds (not waiting for full reply)

---

### Step 11 · Drag & drop PDF upload

**Concept:** The HTML Drag and Drop API lets elements become drop zones. When a user drops a file, you get access to a `File` object — the same as a regular file input. You send it to your `/upload` endpoint using `FormData`. The `react-dropzone` library handles all the browser quirks for you.

**Read:**
- [MDN — HTML Drag and Drop API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API)
- [react-dropzone docs](https://react-dropzone.js.org/)

**Build:**

```
Drag PDF into chat area
  → POST /upload with FormData
  → confirmation message appears in chat
  → user can now ask questions about that doc
```

---

## Phase 7 — Polish & Production Readiness (Week 12+)

> **Goal:** Make it something you'd actually put in a portfolio.

---

### Step 12 · Auth + multi-user sessions

**Concept — JWT recap:** A **JSON Web Token** (JWT) is a signed token that encodes the user's identity (e.g., `{ "user_id": 42, "email": "leticia@..." }`). The client stores it and sends it in every request header. The server verifies the signature and knows who the request is from — without hitting the database every time. You likely already know this from your NestJS work — the concept is identical in FastAPI.

**Why it matters here:** Without auth, all users share the same documents and chat history. With auth, each user has their own isolated data.

**Read:** [FastAPI — Security / OAuth2 with JWT](https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/)

---

### Step 13 · Prompting best practices & versioning

**Concept:** Your system prompt (the instructions that define how the LLM behaves) will change over time as you improve the chatbot. If it's hardcoded in a Python file, every change requires a code deploy. **Prompt versioning** means storing prompts in a config file or DB table with version numbers, so you can:
- A/B test different prompts without deploying
- Roll back if a new prompt performs worse
- Track what changed and when

This comes up frequently in AI engineering interviews because it's a real production concern.

**Read:** [OpenAI — Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)

---

### Step 14 · Deployment

**Concept:** Containerizing with Docker ensures your app runs the same everywhere — on your machine, on a server, in CI. Supabase gives you a managed Postgres instance (with pgvector support built in) on a free tier, so you don't need to run your own database server.

**Read:**
- [FastAPI — Deployment with Docker](https://fastapi.tiangolo.com/deployment/docker/)
- [Supabase](https://supabase.com/) ← managed Postgres + pgvector, free tier, easiest option

---

## 📊 Summary Table

| Phase | What you learn | What you build |
|---|---|---|
| 0 · Core Concepts | LLMs, tokens, context window, chunks, embeddings, vector stores, prompt templates | No code — vocabulary only |
| 1 · Foundations | RAG pipeline, pgvector | Terminal Q&A script |
| 2 · API | FastAPI, document loaders, LangChain chains | REST API with `/upload` + `/chat` |
| 3 · Streaming | SSE, StreamingResponse, tokens in real time | Tokens streamed back token by token |
| 4 · Chat History | Stateless LLMs, message persistence, context reconstruction | Conversations saved & reloaded from Postgres |
| 5 · LangGraph | Graphs, nodes/edges, state, checkpointing, observable pattern | Graph-based RAG with auto-persistence |
| 6 · Frontend | ReadableStream, React streaming UI, drag & drop | Chat UI + drag & drop PDF upload |
| 7 · Production | Auth/JWT, prompt versioning, Docker, deployment | Portfolio-ready project |

---

## 🎯 Interview Topics Coverage (Matheus)

| Topic mentioned | Where it's covered |
|---|---|
| Streaming | Phase 3 — Step 5 |
| Observable pattern / save messages on node end | Phase 5 — Steps 8 & 9 |
| Frontend with streaming | Phase 6 — Step 10 |
| Reconstruct chat from DB | Phase 4 — Step 7 |
| Postgres | Steps 2, 6, 7, 9 |
| Drag & drop → vector DB in real time | Phase 6 — Step 11 |
| RAG over documents | Phases 1 & 2 |
| Memory management & context | Concept 0.3 + Step 7 |
| Prompting versioning | Step 13 |
| Design patterns | LangGraph (Steps 8–9) |
| DSA | ⚠️ Practice separately on LeetCode — not part of this project |

---

> **Tip:** Don't skip Phase 0. The concepts there are the vocabulary of every conversation you'll have about this project — in interviews, in code reviews, and when reading documentation. If you skip it, Phase 1 will feel like reading a foreign language. If you do it, Phase 1 will feel obvious.