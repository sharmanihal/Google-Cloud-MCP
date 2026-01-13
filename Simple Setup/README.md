## Simple (Native) Setup – OS-Independent

The **simple setup** runs MCP servers **natively using Node.js** and **does not require Docker**.

✅ **Same `mcp.json` works on Windows, macOS, and Linux**
✅ No path mapping required
✅ Uses your existing `gcloud` installation directly

---

## Requirements

* Node.js **v20+**
* Google Cloud CLI
* Authenticated with gcloud

```bash
gcloud auth login
gcloud auth application-default login
gcloud config set project PROJECT_ID
```

---

## MCP Configuration (`mcp.json`)

This file is **identical across all operating systems**. Use this with a LLM that supports MCP Servers like Claude Code Desktop, Github Copilot, Gemini CLI etc.

```json
{
  "servers": {
    "gcloud-mcp": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@google-cloud/gcloud-mcp"]
    },
    "observability-mcp": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@google-cloud/observability-mcp"]
    },
    "storage-mcp": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@google-cloud/storage-mcp"]
    }
  },
  "inputs": []
}
```

---

## Why This Works on All OSes

* MCP servers run as **Node.js processes**
* They invoke the locally installed **gcloud CLI**
* gcloud already knows where credentials live:

  * Windows: `%APPDATA%\gcloud`
  * macOS/Linux: `$HOME/.config/gcloud`

👉 MCP **does not need to know or configure these paths**

---

## How MCP Auth Works (Simple Setup)

* You log in once using `gcloud`
* MCP uses the **active gcloud context**
* IAM permissions are fully respected
* No keys or secrets are created
