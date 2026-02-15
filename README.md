# 🚀 Build-an-Enhanced-MCP-Server

[![MCP](https://img.shields.io/badge/Protocol-MCP-blueviolet)](https://modelcontextprotocol.io)
[![Python](https://img.shields.io/badge/Language-Python-blue)](https://www.python.org)
[![Status](https://img.shields.io/badge/Status-Completed-success)](#)

Welcome to the **Enhanced MCP (Model Context Protocol) Server** project. This repository demonstrates how to build a universal bridge between AI models and their environment, enabling them to interact with local files, run tools, and follow complex workflows.

---

## 🌟 Key Features

The server implements the three core **MCP Primitives** to extend LLM capabilities:

### 🛠️ Tools (Active Operations)
* **File Creation**: A function that creates a file given the file path name and contents, including automatic parent directory creation.
* **File Deletion**: A tool that checks if a path is a file and safely deletes it.
* **Progress Reporting**: Uses MCP context to report the status of operations in chunks and logs error messages.

### 📂 Resources (Data Access)
* **File Inspector**: A resource template using the `file:///` URI system to fetch and read specific file contents.
* **Directory Navigator**: Lists all files and directories in the current directory, including their metadata, returned in a JSON object.

### 📝 Prompts (Guided Workflows)
* **Code Reviewer**: A structured template designed to perform code review and quality evaluation on a given code file.
* **Auto-Documentation**: Leverages **User Elicitation** via MCP context to request the target file and new documentation name from the user.

---

## 🔧 Installation & Setup

Follow these steps to get your enhanced server running in the virtual environment:

1. **Clone the Repo**
   ```bash
   git clone [https://github.com/Leelaissakattaota/Build-an-Enhanced-MCP-Server.git](https://github.com/Leelaissakattaota/Build-an-Enhanced-MCP-Server.git)
   cd Build-an-Enhanced-MCP-Server
