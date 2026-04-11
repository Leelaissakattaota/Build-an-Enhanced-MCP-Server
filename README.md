# 🛠️ Build an Enhanced MCP Server with Python

![Language](https://img.shields.io/badge/Language-Python%203.11-3776AB?style=flat-square&logo=python&logoColor=white)
![Framework](https://img.shields.io/badge/Framework-FastMCP%202.12.4-0052CC?style=flat-square)
![Protocol](https://img.shields.io/badge/Protocol-Model%20Context%20Protocol-6A0DAD?style=flat-square)
![LLM](https://img.shields.io/badge/LLM-Claude%20Sonnet%204.5-D97706?style=flat-square)
![Transport](https://img.shields.io/badge/Transport-STDIO-00897B?style=flat-square)
![Focus](https://img.shields.io/badge/Focus-Enhanced%20MCP%20Server-2E7D32?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)

---

## 📌 Project Overview

This project builds an **enhanced, production-grade MCP server** 
with advanced features beyond basic tool calling — including 
**real-time progress reporting**, **context-aware logging**, 
**elicitation-based user input**, and **Claude Sonnet integration** 
via the Anthropic API.

It demonstrates how to build MCP servers that can interact 
intelligently with Claude, handle file operations securely, 
and collect structured user input through elicitation workflows.

**Domain:** Agentic AI — Enhanced MCP Server Development  
**Language:** Python 3.11  
**LLM:** Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`)  
**Transport:** STDIO  

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────┐
│              MCP Client (client.py)               │
│                                                  │
│  MCPClient Class                                 │
│  ├── Anthropic Claude Sonnet 4.5 integration     │
│  ├── elicitation_handler → user input dialogs    │
│  ├── progress_handler → real-time progress       │
│  ├── Tool execution via FastMCP Client           │
│  └── Interactive chat loop with Claude           │
└─────────────────────┬────────────────────────────┘
                      │  STDIO Transport
┌─────────────────────▼────────────────────────────┐
│         Enhanced MCP Server (server.py)           │
│                                                  │
│  🔧 Tools (with Context + Progress)              │
│  ├── write_file(path, content, ctx)              │
│  │   → Chunked writing + progress reporting      │
│  └── delete_file(path, ctx)                      │
│      → Validated deletion with warnings          │
│                                                  │
│  📦 Resources                                    │
│  ├── file:///{file_name}  → Read any file        │
│  └── dir://.             → List directory        │
│                                                  │
│  💬 Prompts (with Elicitation)                   │
│  ├── code_review(file_path, ctx)                 │
│  │   → Reads file → generates review prompt     │
│  └── documentation_generator(ctx)               │
│      → Elicits user input → generates doc prompt │
│                                                  │
│  🛡️ Security: get_path() — BASE_DIR validation   │
└──────────────────────────────────────────────────┘
```

---

## 📂 Project Structure

```
Build-an-Enhanced-MCP-Server/
│
├── server.py            # Enhanced FastMCP server
├── client.py            # Claude-integrated MCP client
├── requirements.txt     # Dependencies
├── .gitignore
└── README.md
```

---

## 🛠️ Tech Stack

| Component | Technology | Version |
|---|---|---|
| MCP Framework | FastMCP | 2.12.4 |
| MCP Protocol | Model Context Protocol | 1.15.0 |
| Language | Python | 3.11+ |
| LLM | Claude Sonnet 4.5 | anthropic 0.69.0 |
| Config | python-dotenv | 1.1.1 |
| Data Validation | Pydantic BaseModel | Built-in |
| Transport | STDIO | stdin/stdout |

---

## 🌟 Enhanced Features (vs Basic MCP Server)

### ✅ 1. Real-Time Progress Reporting
Tools report live progress during long operations:

```python
await ctx.report_progress(
    progress=written,
    total=total,
    message=f"Writing progress: {written}/{total}"
)
```

### ✅ 2. Context-Aware Logging
Every operation logs at appropriate levels:

```python
await ctx.info("File written successfully")
await ctx.warning("File not found: path")
await ctx.error("Error creating file: msg")
```

### ✅ 3. Elicitation — Structured User Input
Server requests structured user input via Pydantic schema:

```python
class DocumentGeneratorSchema(BaseModel):
    file_path: str   # File to document
    name: str        # Output documentation filename

result = await ctx.elicit(
    message="Please provide the subject file name and documentation file name",
    response_type=DocumentGeneratorSchema
)
```

### ✅ 4. Claude Sonnet 4.5 Integration
Client uses Anthropic API directly with MCP tools:

```python
MODEL_ID = "claude-sonnet-4-5-20250929"
self.anthropic = Anthropic()
```

### ✅ 5. Path Security (get_path)
All file operations validated against BASE_DIR:

```python
def get_path(relative_path: str) -> Path:
    rel = Path(relative_path).resolve().relative_to(BASE_DIR)
    return rel  # Raises ValueError if outside BASE_DIR
```

---

## 🔧 Server Capabilities

### Tools

| Tool | Arguments | Enhanced Feature |
|---|---|---|
| `write_file` | `file_path, content, ctx` | Chunked writing + progress reporting |
| `delete_file` | `file_path, ctx` | Validates file vs directory + warns |

### Resources

| URI | Description |
|---|---|
| `file:///{file_name}` | Read any file — returns content or error dict |
| `dir://.` | List directory — returns name, path, type, size, timestamps |

### Prompts

| Prompt | Arguments | Special Feature |
|---|---|---|
| `code_review` | `file_path, ctx` | Reads file + detects language from extension |
| `documentation_generator` | `ctx` | Elicits file_path + doc_name from user |

---

## ⚙️ How to Run

**Step 1 — Install dependencies:**
```bash
pip install -r requirements.txt
```

**Step 2 — Set up environment variables:**
```bash
# Create .env file
echo "ANTHROPIC_API_KEY=your-api-key-here" > .env
```

**Step 3 — Run the client (connects to server automatically):**
```bash
python client.py server.py
```

---

## 🔄 Elicitation Workflow

```
User asks: "Generate documentation for server.py"
           │
           ▼
Claude calls: documentation_generator prompt
           │
           ▼
Server triggers ctx.elicit():
"Please provide subject file and documentation file name"
           │
           ▼
Client elicitation_handler shows dialog to user
User inputs: {file_path: "server.py", name: "server_docs"}
           │
           ▼
Server reads server.py → builds documentation prompt
           │
           ▼
Claude receives prompt → calls write_file tool
           │
           ▼
Server writes server_docs.md with progress reporting
           │
           ▼
Documentation file created! ✅
```

---

## 🎓 Skills Demonstrated

- Enhanced MCP server with Context (ctx) integration
- Real-time progress reporting via `ctx.report_progress()`
- Multi-level context logging — info, warning, error
- Pydantic-based elicitation schema design
- User-in-the-loop structured input collection
- Claude Sonnet 4.5 integration via Anthropic API
- MCP client with elicitation and progress handlers
- FastMCP Client with custom event handlers
- Secure path validation with BASE_DIR boundary
- Async file operations with chunked writing
- Directory listing with full file metadata
- Language detection from file extensions
- Dynamic prompt generation from file contents

---

## 📜 Certifications

| Certification | Issuer | Platform |
|---|---|---|
| IBM Data Science Professional Certificate | IBM | Coursera |
| IBM Generative AI Professional Certificate | IBM | Coursera |
| IBM Agentic AI with RAG Certificate | IBM | Coursera |
| IBM RAG and Agentic AI Professional Certificate | IBM | Coursera |

---

## 🤝 Connect with Me

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Leela%20A-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/leela-a)
[![Gmail](https://img.shields.io/badge/Gmail-attotaleelaissak@gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:attotaleelaissak@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-Leelaissakattaota-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/Leelaissakattaota)
