# Google-Cloud-MCP


## Installation Guide

**Node.js & Docker (Step-by-Step)**

This project supports **two execution modes**:

1. **Native mode** → Requires **Node.js**
2. **Docker mode (optional)** → Requires **Docker**


---

## What Should Be Installed First?

### Recommended Order

| Order | Tool                  | Why                                       |
| ----- | --------------------- | ----------------------------------------- |
| 1     | **Node.js**           | Required for native MCP servers and `npx` |
| 2     | **Docker** (optional) | Used only for containerized MCP servers   |
| 3     | **gcloud CLI**        | Required for authentication & permissions |

---

## 1️⃣ Install Node.js (Required)

Node.js is required to run MCP servers using `npx`.

### Required Version

* **Node.js v20 or newer**

---

### Windows

1. Download Node.js LTS from:
   [https://nodejs.org](https://nodejs.org)
2. Run the installer
3. Keep **“Add to PATH”** enabled
4. Finish installation

Verify:

```powershell
node -v
npm -v
```

---

### macOS

Using Homebrew (recommended):

```bash
brew install node
```

Verify:

```bash
node -v
npm -v
```

---

### Linux (Debian / Ubuntu)

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Verify:

```bash
node -v
npm -v
```

---

## 2️⃣ Install Docker (Optional but Recommended)

Docker is **optional** and only required if you want to run MCP servers in containers.

### When should you install Docker?

* Reproducible demos
* Strong isolation

---

### Windows

1. Download **Docker Desktop**:
   [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
2. Install and restart
3. Ensure **Linux containers** are enabled

Verify:

```powershell
docker --version
docker run hello-world
```

---

### macOS

```bash
brew install --cask docker
```

Open Docker Desktop once after installation.

Verify:

```bash
docker --version
docker run hello-world
```

---

### Linux

```bash
sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
```

Verify:

```bash
docker --version
docker run hello-world
```

---

## 3️⃣ Install Google Cloud CLI (Required)

The MCP servers rely on **your existing gcloud authentication**.

---

### Windows

```powershell
winget install --id Google.CloudSDK
```

Restart terminal.

---

### macOS

```bash
brew install --cask google-cloud-sdk
```

---

### Linux

```bash
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
```

---

Verify:

```bash
gcloud version
```

---

## 4️⃣ Authenticate with Google Cloud

This is **mandatory** for MCP to access your GCP environment.

```bash
gcloud init
gcloud auth login
gcloud auth application-default login
gcloud config set project PROJECT_ID
```

---

## 5️⃣ Verify Everything Is Ready

Run all of the following successfully:

```bash
node -v
npm -v
gcloud version
```

(Optional, Docker users)

```bash
docker --version
```

---

## 6️⃣ Next Steps

Choose how you want to run MCP servers:

| Mode                 | Next Step                |
| -------------------- | ------------------------ |
| Native (recommended) | Go to `Simple Setup/README.md` |
| Docker-based         | Go to `Docker Setup/README.md` |

---

## Common Mistakes to Avoid

❌ Skipping `gcloud auth application-default login`
❌ Using Node < v20
❌ Forgetting to restart terminal after installs

---

## Summary

✅ Node.js → **Required**
✅ Docker → **Optional**
✅ gcloud → **Required**
✅ Auth → **Mandatory**

Once installed, your AI assistant can safely and directly interact with **real GCP infrastructure** using MCP.
