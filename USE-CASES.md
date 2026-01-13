## What Can Google Cloud MCP Act As?

When MCP servers are connected, your AI assistant is no longer just a chatbot — it becomes a **context-aware cloud operator**.

Below are **realistic roles MCP can play**, along with **example prompts** you can actually use.

---

## 💰 Cost Optimization Assistant

MCP can act as a **continuous cost hygiene and optimization tool** by inspecting live infrastructure, storage, and usage patterns.

### Storage Cost Optimization (GCS)

**Prompts**

* “List GCS buckets without lifecycle rules and estimate potential cost impact.”
* “Which buckets have versioning enabled but no lifecycle cleanup?”
* “Find buckets storing objects older than 180 days in STANDARD storage.”
* “Which buckets are public and also storing large objects?”

**What MCP does**

* Uses **Storage MCP** to inspect bucket metadata
* Cross-references lifecycle rules and versioning
* Explains *why* cost is higher and *what to change*

---

### Dataflow Cost Optimization

**Prompts**

* “List all running Dataflow jobs and their worker types.”
* “Which Dataflow jobs are using autoscaling but still running at max workers?”
* “Identify idle or long-running Dataflow jobs with low throughput.”
* “Compare Dataflow worker usage between last week and today.”

**What MCP does**

* Uses **gcloud MCP** for job metadata
* Uses **Observability MCP** for metrics
* Surfaces inefficiencies instead of raw numbers

---

### Compute / GKE Cost Checks

**Prompts**

* “Find underutilized GKE node pools.”
* “Which services are running in production but have near-zero traffic?”
* “List VM instances without recent CPU activity.”

---

## 🧱 Infrastructure Configuration Checker (Terraform / IaC Validation)

MCP can act as an **infra drift and deployment verification tool**.

### Terraform Deployment Validation

**Prompts**

* “Compare deployed GCP resources with what Terraform defines.”
* “Which GCS buckets exist in the project but are not managed by Terraform?”
* “Did the last Terraform apply successfully create all expected resources?”
* “Check if IAM bindings match what’s defined in Terraform.”

**What MCP does**

* Uses **gcloud MCP** to inspect real infra
* Flags drift, missing resources, or misconfigurations
* Explains *what exists vs what should exist*

> ⚠️ MCP does **not** run Terraform — it validates **outcomes**, not plans.

---

### Environment Consistency (Dev / Staging / Prod)

**Prompts**

* “Compare IAM roles between staging and production.”
* “Are the same APIs enabled in dev and prod?”
* “Compare GKE cluster versions across environments.”

---

## 🚨 Incident & Failure Investigation (SRE Mode)

MCP shines during incidents — it replaces dashboard hopping with **guided debugging**.

---

### Dataflow Job Failures

**Prompts**

* “Why did this Dataflow job fail in the last hour?”
* “Show recent error logs for this Dataflow job.”
* “Was the failure caused by input data, worker exhaustion, or permissions?”
* “Did this job fail after a deployment or config change?”

**What MCP does**

* Pulls logs via **Observability MCP**
* Correlates timestamps, errors, and job state
* Suggests *probable root cause* and next steps

---

### End-to-End Request Failures (GCP Equivalent of E2E)

In GCP, this usually means **Cloud Load Balancer → Cloud Run / GKE → Backend → Storage / PubSub**.

**Prompts**

* “Trace this request end-to-end and identify where it failed.”
* “Which service introduced the highest latency in this request?”
* “Show logs and traces associated with this trace ID.”
* “Did error rates increase after the last deployment?”

**What MCP does**

* Uses **Observability MCP** (logs + traces + metrics)
* Follows the request path across services
* Identifies bottlenecks and failure points

---

## 📊 Proactive Monitoring & Alert Analysis

MCP can act as a **monitoring analyst**, not just a log viewer.

**Prompts**

* “Why did this alert fire?”
* “Has this alert fired before? What was the cause then?”
* “Is this alert noise or a real issue?”
* “Which metrics crossed thresholds leading to this alert?”

---

## 🔐 Security & Compliance Assistant

**Prompts**

* “List service accounts with overly broad permissions.”
* “Which GCS buckets are public?”
* “Find IAM bindings that violate least-privilege principles.”
* “Show recently changed IAM policies.”

**What MCP does**

* Uses **gcloud MCP + Storage MCP**
* Flags risky configurations
* Explains blast radius, not just permissions

---

## 🧠 Architecture Understanding & Onboarding

For engineers new to a project:

**Prompts**

* “Explain the architecture of this GCP project.”
* “Which services talk to each other?”
* “What are the critical production dependencies?”
* “Which components are most failure-prone?”

MCP turns **existing infra into documentation**.

---

## 🛠️ Operational Decision Support

**Prompts**

* “What will be impacted if this service goes down?”
* “Which downstream systems depend on this Dataflow job?”
* “Is it safe to restart this service right now?”

---

## Why This Is Powerful

Without MCP:

* Raw dashboards
* Manual correlation
* High cognitive load

With MCP:

* Live data
* Structured reasoning
* Context-aware answers
* Faster root cause analysis

The AI doesn’t guess — it **queries, correlates, and explains**.

