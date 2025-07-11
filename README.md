# 🚀 API Ops with decK

This repository demonstrates a complete **API Ops** pipeline using [Kong](https://konghq.com) and [decK](https://deck.run). It includes OpenAPI-driven deployments, GitHub Actions automation, plugin management, and environment-safe GitOps practices.

---

## 📘 What is API Ops?

**API Ops** (API Operations) is the application of **DevOps principles to the lifecycle of APIs**. It promotes:

- ✅ Version-controlled API definitions
- ✅ Automated deployments through CI/CD
- ✅ Testing, validation, and promotion across environments
- ✅ Seamless collaboration between devs, ops, and security teams

Think of it as *GitOps for your APIs* — reproducible, auditable, and automated.

---

## 🔧 How decK Helps in API Ops?

[decK](https://deck.run) is Kong's declarative configuration tool.
decK communicates with Kong Gateway via the Admin API. 
It enables:

- Converting OpenAPI specs to Kong-native configurations
- Validating and syncing configuration state with Kong Gateway
- Managing plugins, consumers, services, and routes as code
- Supporting Git-based workflows with dry-run previews

---

## ❓ Why decK?

| Feature           | Benefit                                  |
|------------------|-------------------------------------------|
| YAML-based state  | Human-readable, version-controlled        |
| CLI-friendly      | Easily fits into GitHub Actions & CI/CD   |
| Diffing & Sync    | Ensures no drift between Git and Gateway  |
| Plugin Automation | Add, tag, and patch plugins easily        |
| Extensible        | Supports patches, merges, and rendering   |

---

## 🛠️ Common decK Commands Used

```bash
# Convert OpenAPI spec to Kong config
deck file openapi2kong 

# Validate a deck YAML file
deck file validate 

# Add plugins declaratively
deck file add-plugins 

# Linting
deck file lint

# Add tags
deck file add-tags 

# Patch fields (like retries, timeouts, etc.)
deck file patch 

# Merge multiple deck files
deck file merge 

# Render final state from templates or inputs
deck file render 

# Gateway-specific commands
deck gateway ping
deck gateway validate
deck gateway dump
deck gateway diff 
deck gateway sync
```

---

## 🧾 Explore: YAML State File Structure

A typical `deck.yaml` (generated via `openapi2kong`) contains:

```yaml
_format_version: "3.0"
services:
  - name: flights-service
    url: http://example.com
    routes:
      - name: flights-get
        paths:
          - /flights
plugins:
  - name: rate-limiting
    config:
      minute: 10
      policy: local
consumers:
  - username: jdoe
    keyauth_credentials:
      - key: jdoe-key
```

---

## 🐞 Troubleshooting decK

Deck makes a sequence of API calls to sync with Konnect
You can view these underlying API requests/responses by appending the **--verbose=2** flag

| Issue                            | Cause / Fix |
|----------------------------------|-------------|
| Plugin not applied               | Check selector path or missing `name` in target entity |
| `Validation failed`              | Run `deck file validate --input <file>` to catch schema issues |
| `Could not resolve to a node`    | Happens if a PR or label doesn’t exist (in GitHub Actions) |
| File not saved after command     | Use `--output <file>` to persist the result |
| Gateway ping fails               | Check Kong Admin URL & Auth token (`KONG_ADMIN_URL`, `KONG_ADMIN_TOKEN`) |
| YAML structure missing metadata  | Ensure `_format_version` is `3.x` and all services/routes have `name` fields |

---

## 📂 Folder Overview

```bash
.
├── flights/openapi/                # OpenAPI spec input
├── flights/kong/                   # Kong config files
├── .github/.generated/             # Output folder for processed final config files
├── .github/workflows/              # GitHub Actions pipeline
└── README.md                       # This file
```

---

## decK useful links:
[decK commands](https://developer.konghq.com/index/deck/#overview)
[APIOPS Example](https://developer.konghq.com/deck/apiops/#an-apiops-example)

