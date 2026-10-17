# **OpenAI Agents SDK Series**

Welcome to the **OpenAI Agents SDK Playlist** repository!  

This playlist is designed to help you understand the `openai-agents` Python SDK from the ground up — starting with your first `Agent`/`Runner` call, all the way to advanced topics like handoffs, guardrails, sessions, human-in-the-loop, MCP, tool search, realtime agents, tracing, and a production FastAPI capstone.  
Each video walks you through real, practical examples — set in the Naruto universe — so you can build production‑ready AI agents on the same SDK that powers real OpenAI agent deployments.

---

## 🐍 **Install Python Using Miniconda / Miniforge**

To keep your AI projects clean and organized, it is recommended to use **conda environments**. Follow the steps below to install Miniforge and set up your environment.

---

### 🔗 **Download Miniforge for macOS (ARM64)**

Download from the official repository:  
https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh

---

### 💻 **Install Miniforge**

Run the following commands:

```bash
chmod +x ~/Downloads/Miniforge3-MacOSX-arm64.sh
sh ~/Downloads/Miniforge3-MacOSX-arm64.sh
source ~/miniforge3/bin/activate
```

---

### 🧱 **Create a project-specific conda environment**

```bash
conda create --prefix ./.openai-agent-sdk-youtube-env python=3.12
conda activate ./.openai-agent-sdk-youtube-env
```

---

### 📦 **Install packages from requirements.txt**

```bash
pip install -r requirements.txt
```

---

### 🔑 **Add your OpenAI API key**

Add a `.env` file in the project root with your key (it's git-ignored, so it stays local):

```
OPENAI_API_KEY=your-key-here
```

Every notebook runs end to end without a key — live-API cells detect a missing `OPENAI_API_KEY` and skip gracefully, falling back to `agents.testing.ScriptedModel` for deterministic, offline demos.

Your agents environment is ready to build powerful AI apps 🚀

---

# **📺 Playlist Breakdown**

### **1. Setup & Your First Agent**

- Understanding that an `Agent` is only configuration — nothing happens until a `Runner` executes it.
- Running the same agent three ways: `Runner.run_sync`, `Runner.run`, and `Runner.run_streamed`.

### **2. Instructions, ModelSettings & Structured Output**

- Writing dynamic `instructions` that adapt the agent's persona to who's asking.
- Turning free-text answers into validated Pydantic objects with `output_type` and tuning them with `ModelSettings`.

### **3. Function Tools**

- Giving an agent real Python tools with `@function_tool` and a `RunContextWrapper`.
- Constraining tool schemas with `Annotated`/`Field`, plus tool timeouts and custom failure messages.

### **4. Hosted Tools**

- Using OpenAI's server-side tools: web search, file search, code interpreter, and image generation.
- Separating what can be verified offline (construction and config) from what needs a live key.

### **5. RunResult, Streaming & Max Turns**

- Exploring everything a `RunResult` carries beyond `final_output`.
- Reading streamed events one level deeper and capping runs with `max_turns`.

### **6. Refusals & Errors**

- Detecting genuine model refusals with `ModelRefusalError`.
- Recovering from run-ending exceptions with `error_handlers` instead of letting them propagate.

### **7. Timeouts & Retries**

- Capping a single model call with `ModelSettings(timeout=...)`.
- Configuring `ModelRetrySettings` with an explicit retry policy.

### **8. Handoffs**

- Routing requests from a Mission Desk agent to specialist agents with `handoff()`.
- Controlling side effects and history with `on_handoff`, `input_filter`, and `nest_handoff_history`.

### **9. Agents-as-Tools**

- Calling a specialist agent like a tool with `Agent.as_tool()` while the original agent stays in control.
- Comparing agents-as-tools against handoffs and customizing input with `input_builder`.

### **10. Guardrails**

- Defending three layers: input, output, and individual tool calls.
- Allowing, rejecting, or raising on tool calls, and running input guardrails in parallel with the model.

### **11. Context & Sessions**

- Persisting conversation history with `SQLiteSession`, `SQLAlchemySession`, and `EncryptedSession`.
- Understanding how `RunContextWrapper` context differs from saved chat history.

### **12. Human-in-the-Loop**

- Pausing risky tool calls with `needs_approval` until a human approves or rejects them.
- Serializing a paused `RunState` and resuming it later — even in a different process.

### **13. MCP (Model Context Protocol)**

- Connecting an agent to an MCP server over stdio and Streamable HTTP.
- Offloading the MCP connection to OpenAI with `HostedMCPTool`.

### **14. Hosted Tool Search**

- Hiding tools with `defer_loading` and grouping them with `tool_namespace(...)`.
- Letting the model find only the tools it needs with `ToolSearchTool()` — and measuring the token savings.

### **15. Programmatic Tool Calling**

- Letting the model write one small program that calls many tools in a single step.
- Controlling who may call a tool with `allowed_callers`.

### **16. Tracing & Observability**

- Writing a custom `TracingProcessor` and adding your own steps with `custom_span(...)`.
- Keeping private data out of traces with `trace_include_sensitive_data=False`.

### **17. Realtime Agents**

- Building a live, always-open session with `RealtimeAgent` and `RealtimeRunner`.
- Following tool and agent events in a text-only session — no microphone required.

### **18. Testing & Evals**

- Writing fast, free, deterministic agent tests with `ScriptedModel`.
- Building an eval set and running it against both the scripted and the real model.

### **19. Capstone: Hidden Leaf Village Mission Ops Center**

- Combining handoffs, guardrails, sessions, human-in-the-loop, and SSE streaming into one FastAPI service.
- Testing the whole app end to end via `httpx`'s ASGI transport.

---

# **📄 requirements.txt**

```
# Core SDK — pinned exactly, since openai-agents is pre-1.0 and minor bumps can break
# episodes. Verified against PyPI 2026-09-21 (see CLAUDE.md).
openai-agents==0.22.3

# openai-agents requires openai v3 (HTTPX2 migration) — keep in the same major line.
openai>=3.0.0,<4

pydantic>=2.12.2,<3

# .env loading for OPENAI_API_KEY
python-dotenv

# Formatting is handled manually via the user's editor (Black extension) — no ruff or
# black installed/run here.

# Notebook authoring/execution — committed with real outputs intact, not stripped
jupyterlab
ipykernel

# Episode-specific extras are installed on demand rather than globally, to keep early
# episodes' installs fast — see CLAUDE.md.
# Episode 11 (sessions) needs the `sqlalchemy`/`encrypt` extras plus `aiosqlite` and
# `greenlet` for SQLAlchemySession's async engine:
#   pip install "openai-agents[sqlalchemy,encrypt]==0.22.3" aiosqlite greenlet

# Episode 19 (capstone) — the one episode that's a FastAPI app.
fastapi
uvicorn
httpx
```

---

# **🤝 Contributing**

Got suggestions or improvements?  
Spotted a bug or an SDK drift (the SDK is pre-1.0 and moves fast)? Feel free to open an issue or submit a pull request.

---

# **📬 Stay Connected**

- [YouTube Channel](https://www.youtube.com/@yashjainio)
- [LinkedIn](https://www.linkedin.com/in/yashjainio)
- [GitHub](https://github.com/yashjainio)

---

Thank you for checking out the **OpenAI Agents SDK Playlist**!  
Happy building with AI 🚀
