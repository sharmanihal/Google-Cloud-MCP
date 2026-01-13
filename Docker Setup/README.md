# Google Cloud MCP – Docker Setup

This directory contains a **Docker-based setup** for running **Google Cloud MCP servers** using separate containers for each domain:

- **gcloud MCP**
- **Observability MCP**
- **Storage MCP**

This approach is ideal for:
- Windows users
- Reproducible demos
- Strong isolation and least-privilege execution

Each MCP server runs in its **own container**, following best practices.

---

## Directory Structure

```

Docker Setup/
├── Gcloud.Dockerfile
├── Observability.Dockerfile
├── Storage.Dockerfile
└── mcp.json

````

---

## What This Setup Does

- Builds **three independent Docker images**
- Each image runs **one MCP server**
- MCP servers:
  - Use your existing `gcloud` authentication
  - Respect IAM permissions
  - Do **not** store credentials inside containers
- AI tools (Copilot / Claude) communicate with MCP via `stdio`

---

## Prerequisites

Before proceeding, ensure the following are installed:

### Required
- **Docker Desktop** (Linux containers enabled)
- **Google Cloud CLI**

### Authentication (Mandatory)

You must be logged into Google Cloud **before** using MCP:

```bash
gcloud auth login
gcloud auth application-default login
gcloud config set project PROJECT_ID
````

Verify:

```bash
gcloud auth list
gcloud config list
```

---

## Step 1: Build Docker Images

From this directory, run:

### gcloud MCP

```bash
docker build -f Gcloud.Dockerfile -t gcloud-mcp:local .
```

### Observability MCP

```bash
docker build -f Observability.Dockerfile -t observability-mcp:local .
```

### Storage MCP

```bash
docker build -f Storage.Dockerfile -t storage-mcp:local .
```

Verify images:

```bash
docker images | grep mcp
```

---

## Step 2: Configure MCP (`mcp.json`)

This repository includes a ready-to-use `mcp.json`.

🪟 Windows

Replace <username> with your Windows username:
```
C:\Users\<username>\AppData\Roaming\gcloud
```

Example
```
C:\Users\Nihal\AppData\Roaming\gcloud
```

🍎 macOS

Use your home directory:
```
$HOME/.config/gcloud
```

Example Docker volume mapping:
```
-v "$HOME/.config/gcloud:/root/.config/gcloud"
```
🐧 Linux

Use the same path as macOS:
```
$HOME/.config/gcloud
```

Example Docker volume mapping:
```
-v "$HOME/.config/gcloud:/root/.config/gcloud"
```

This directory is mounted into each container so MCP can reuse your
existing gcloud authentication.

---

## MCP Configuration (Final)

### Windows mcp.json
```json
{
  "servers": {
    "gcloud": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "C:\\Users\\<username>\\AppData\\Roaming\\gcloud:/root/.config/gcloud",
        "gcloud-mcp:local"
      ]
    },
    "observability": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "C:\\Users\\<username>\\AppData\\Roaming\\gcloud:/root/.config/gcloud",
        "observability-mcp:local"
      ]
    },
    "storage": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "C:\\Users\\<username>\\AppData\\Roaming\\gcloud:/root/.config/gcloud",
        "storage-mcp:local"
      ]
    }
  },
  "inputs": []
}
```

### MacOS mcp.json
```json
{
  "servers": {
    "gcloud": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "$HOME/.config/gcloud:/root/.config/gcloud",
        "gcloud-mcp:local"
      ]
    },
    "observability": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "$HOME/.config/gcloud:/root/.config/gcloud",
        "observability-mcp:local"
      ]
    },
    "storage": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "$HOME/.config/gcloud:/root/.config/gcloud",
        "storage-mcp:local"
      ]
    }
  },
  "inputs": []
}
```

### Linux mcp.json
```json
{
  "servers": {
    "gcloud": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "$HOME/.config/gcloud:/root/.config/gcloud",
        "gcloud-mcp:local"
      ]
    },
    "observability": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "$HOME/.config/gcloud:/root/.config/gcloud",
        "observability-mcp:local"
      ]
    },
    "storage": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-v",
        "$HOME/.config/gcloud:/root/.config/gcloud",
        "storage-mcp:local"
      ]
    }
  },
  "inputs": []
}

```

---

## Step 3: Enable MCP Servers in Your IDE

### VS Code + GitHub Copilot

1. Open VS Code
2. `Ctrl + Shift + P`
3. Select **“MCP: Open User Configuration”**
4. Paste the `mcp.json`
5. Open **Copilot Chat**
6. Switch to **Agent Mode**
7. Enable all three MCP servers

✅ MCP servers will start **automatically when needed**

---

## Example Questions You Can Ask

* “Which GCS buckets don’t have lifecycle policies?”
* “Why did this Dataflow job fail?”
* “Show logs related to this trace ID.”
* “Compare IAM roles between staging and prod.”
* “Which services are causing high latency?”

The AI will transparently invoke MCP servers to fetch **real GCP data**.

---

## Security Model

This setup is **secure by design**:

* Uses existing `gcloud` credentials
* No service account keys
* No secrets inside containers
* IAM permissions fully enforced
* Containers run only MCP binaries
* Dangerous gcloud commands are blocked by MCP

---
