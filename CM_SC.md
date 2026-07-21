# ConfigMap and Secret

This document explains the `cm.yml` (ConfigMap) and `secret.yml` (Secret) manifests used in this project, and how they're consumed by the app.

## Files

- `cm.yml` — ConfigMap holding non-sensitive configuration
- `secret.yml` — Secret holding sensitive configuration (base64-encoded/stringData)

## Why two separate objects instead of hardcoding values in the Deployment

- **Reusability** — the same config can be referenced by multiple Deployments without duplicating values.
- **Separation of concerns** — config/secrets can be updated independently of the app's Deployment spec.
- **Security** — sensitive values are kept out of the main Deployment YAML (and ideally out of public repos), instead of being hardcoded alongside the rest of the app spec.

## ConfigMap (`cm.yml`)

Stores plain, non-sensitive configuration values.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: test-cm
data:
  db-port: "3306"
```

- Values are stored as plain text.
- Visible in full via `kubectl describe configmap test-cm`.

## Secret (`secret.yml`)

Stores sensitive configuration values.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: test-sc
data:
  db-port: MzMwNg==   # base64-encoded "3306"
```

- Values under `data:` must be base64-encoded.
- Alternatively, `stringData:` can be used to write plain text — Kubernetes encodes it automatically.
- Values are hidden by default in `kubectl describe secret`, though **base64 is not encryption** — it's easily reversible (`echo "<value>" | base64 -d`). Real production secrets need encryption at rest and/or an external secrets manager.

> **Note:** Values in this repo are placeholders used for learning purposes only — not real credentials.

## How they're consumed by the app

Two different delivery methods were used in this project:

### 1. As environment variables

```yaml
env:
  - name: DB_PORT
    valueFrom:
      configMapKeyRef:
        name: test-cm
        key: db-port
```

- Injected once, at container startup.
- Does **not** update automatically if the ConfigMap/Secret changes later — the pod needs to be restarted to pick up new values.
- Read in the app via `os.environ["DB_PORT"]`.

### 2. As a mounted volume (file)

```yaml
volumes:
  - name: db-connection
    configMap:
      name: test-cm
containers:
  - volumeMounts:
      - name: db-connection
        mountPath: /opt
```

- Kubernetes writes each key in the ConfigMap/Secret as a separate file at the mount path (e.g. `/opt/db-port`).
- **Does** reflect updates — kubelet periodically syncs the mounted files if the source ConfigMap/Secret changes, without needing a pod restart.
- Read in the app via `open("/opt/db-port")`.

## ConfigMap vs Secret — quick reference

| | ConfigMap | Secret |
|---|---|---|
| Purpose | Non-sensitive config | Sensitive data |
| Encoding | Plain text | Base64 |
| Encrypted by default | No | No (base64 ≠ encryption) |
| Visible via `kubectl describe` | Full value | Hidden/redacted |
| Example | `db-port`, log level | Passwords, API keys |

## Naming rule to remember

Kubernetes object names (ConfigMap/Secret/Volume names, etc.) must be lowercase and can only use letters, numbers, and hyphens — **no underscores** (e.g. `db-connection`, not `db_connection`). Environment variable names are the opposite — they conventionally use underscores (e.g. `DB_PORT`), following normal shell/programming conventions.
