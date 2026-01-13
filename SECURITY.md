## 🔐 Security: Running MCP with Least Privilege (Service Account Impersonation)

Security is a first-class concern when giving AI assistants access to cloud infrastructure.
Google designed **gcloud MCP** to work safely with **service account impersonation**, avoiding long-lived keys and excessive permissions.


---

### 🎯 Goal

* Your **human user** authenticates normally using `gcloud auth login`
* MCP runs **as a restricted service account**
* The service account has **only minimum required permissions**
* **No service account keys** are created
* MCP uses **impersonation**, fully auditable via IAM
* Dangerous or interactive gcloud commands are blocked by MCP itself

This follows **least privilege + no secrets** best practices.

---

## High-Level Flow

1. Create a service account
2. Grant it limited (viewer-level) roles
3. Allow your user to impersonate it
4. Configure gcloud to use impersonation
5. MCP automatically inherits this behavior

---

## 1. Create a Service Account

```bash
gcloud iam service-accounts create mcp-sa \
  --description="Service account for gcloud MCP access" \
  --display-name="mcp-service-account"
```

Service account email:

```
mcp-sa@PROJECT_ID.iam.gserviceaccount.com
```

---

## 2. Grant Least-Privilege Roles

Only grant what MCP needs.

### Read-only infrastructure access

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="serviceAccount:mcp-sa@PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/viewer"
```

### Google Cloud Storage inspection

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="serviceAccount:mcp-sa@PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/storage.viewer"
```

### Logs, metrics, and traces

```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="serviceAccount:mcp-sa@PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/logging.viewer"

gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="serviceAccount:mcp-sa@PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/monitoring.viewer"
```

🚫 **Do not grant `Owner` or `Editor` roles**

---

## 3. Allow User to Impersonate the Service Account

This is the **critical step**.

```bash
gcloud iam service-accounts add-iam-policy-binding \
  mcp-sa@PROJECT_ID.iam.gserviceaccount.com \
  --member="user:YOUR_EMAIL@gmail.com" \
  --role="roles/iam.serviceAccountTokenCreator"
```

This allows your identity to **temporarily act as the service account**.

✅ No key files
✅ No secrets stored
✅ Fully auditable

---

## 4. Configure gcloud to Use Impersonation

Set this once:

```bash
gcloud config set auth/impersonate_service_account \
  mcp-sa@PROJECT_ID.iam.gserviceaccount.com
```

Verify:

```bash
gcloud config list
```

Expected output:

```
auth/impersonate_service_account = mcp-sa@PROJECT_ID.iam.gserviceaccount.com
```

---

## 5. Test Least Privilege (Important)

Allowed action:

```bash
gcloud projects describe PROJECT_ID
```

Disallowed action (should fail):

```bash
gcloud projects delete PROJECT_ID
```

❌ Failure here is **expected and desired**.

---

## 6. How This Affects gcloud MCP

Once impersonation is enabled:

* gcloud MCP automatically uses the active gcloud config
* All MCP commands execute:

  * As the **service account**
  * With **limited permissions**
* No MCP configuration changes are required

When Copilot / Claude invokes MCP:

* It cannot exceed the service account’s permissions
* It cannot modify auth or config
* It cannot run interactive commands

---

## 🚫 Blocked Commands in gcloud MCP

By design, gcloud MCP **hard-blocks**:

* `gcloud auth login`
* `gcloud init`
* `gcloud config set`
* Interactive prompts
* Arbitrary shell execution

This protection is **built-in and non-configurable**, which is intentional.

---

## Disable Impersonation (Optional)

To revert to your normal user identity:

```bash
gcloud config unset auth/impersonate_service_account
```

---

## ✅ Recommended Production Pattern

* User logs in normally
* MCP runs via impersonated service account
* Service account has only:

  * `roles/viewer`
  * `roles/storage.viewer`
  * `roles/logging.viewer`
  * `roles/monitoring.viewer`
* No keys
* Fully auditable

