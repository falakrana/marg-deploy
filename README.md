# Marg MCP — Self-Hosted Deployment

> Official self-hosted deployment stack for **Marg MCP**: Enterprise-grade observability, distributed tracing, and diagnostic intelligence for AI agents.

[![Docker Image](https://img.shields.io/badge/Docker-GHCR-blue?logo=docker)](https://github.com/falakrana?tab=packages)
[![Python SDK](https://img.shields.io/pypi/v/marg-sdk.svg?color=blue&logo=pypi)](https://pypi.org/project/marg-sdk/)
[![License](https://img.shields.io/badge/License-Apache--2.0-green.svg)](LICENSE)

---

## ⚡ Quickstart (1 Command)

Start the entire platform (PostgreSQL, Backend API, and Web Dashboard) in the background:

```bash
docker compose up -d
```

That's it! Your platform is now running:
- 🖥️ **Web Dashboard**: [http://localhost:80](http://localhost:80)
- 🔌 **Ingestion API**: `http://localhost:8000`
- 📚 **Swagger Docs**: [http://localhost:8000/docs](http://localhost:8000/docs)

To stop the platform:
```bash
docker compose down
```

---

## 🤖 Connect Your AI Agents (Python)

### 1. Install the SDK
In your AI project / virtual environment:
```bash
pip install marg-sdk
```

### 2. Instrument Your Agent Code

```python
import os
from marg_sdk import Marg, observe_agent, observe_tool, observe_llm

# Point SDK to your self-hosted Marg instance
os.environ["MARG_API_URL"] = "http://localhost:8000"
os.environ["MARG_PROJECT_ID"] = "default"
os.environ["MARG_API_KEY"] = "default_secret_key_change_in_production_32chars"

marg = Marg()

@observe_tool(tool_name="web_search")
def search(query: str):
    return f"Results for: {query}"

@observe_llm(model="gpt-4o")
def call_llm(prompt: str):
    return "Analysis complete."

@observe_agent(agent_name="research-agent")
def run_agent(task: str):
    search(task)
    return call_llm(task)

if __name__ == "__main__":
    run_agent("Evaluate market trends for AI agents")
    print("Trace delivered! Open http://localhost:80 to inspect.")
```

---

## ⚙️ Configuration (Optional)

To customize ports, passwords, or enable the AI Diagnostic Assistant:

1. Copy the environment template:
   ```bash
   cp .env.example .env
   ```
2. Edit `.env` to configure your values:
   - **Custom Ports**: `MARG_HTTP_PORT=3000`, `MARG_PORT=8000`
   - **AI Diagnostic Assistant**: Set `DIAGNOSTIC_LLM_PROVIDER=groq` and `DIAGNOSTIC_LLM_API_KEY=gsk_...`
   - **External Database**: Supply your own Neon, Supabase, or AWS RDS URL in `MARG_DATABASE_URL`.
3. Restart containers:
   ```bash
   docker compose up -d
   ```
