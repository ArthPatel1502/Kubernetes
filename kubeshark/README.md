# Minikube Deployment with Kubeshark Traffic Monitoring

A hands-on project deploying a simple Python REST API to a local Kubernetes cluster (Minikube), exposing it via a NodePort Service, and inspecting live traffic between pods using Kubeshark.

## What this project demonstrates

- Containerizing a Python REST API with Docker
- Deploying it to Kubernetes using a Deployment (with ReplicaSets for self-healing)
- Exposing the app to the outside world with a NodePort Service
- Running a local single-node cluster with Minikube
- Capturing and inspecting live in-cluster network traffic with Kubeshark
- Debugging real networking issues specific to running Kubernetes on macOS

## Architecture

```
kubectl (your machine)
        │
        ▼
Minikube node (single-node cluster)
        │
   Control plane (API server, scheduler)
        │
   Deployment (python-deployment-app)
        │
   ReplicaSet — keeps 2 pod replicas running
        │
   ┌────┴────┐
   Pod        Pod
 (container) (container)
        │
   Service (NodePort) ── exposes pods on a fixed port (e.g. 30007)
```

## Tech stack

- **Docker** — containerizes the Python REST API
- **Kubernetes (Minikube)** — orchestrates the deployment locally
- **kubectl** — CLI to manage the cluster
- **Kubeshark** — captures and visualizes live traffic between pods

## Project structure

```
.
├── app/                  # Python REST API source code
├── Dockerfile            # Container image definition
├── deployment.yaml        # Kubernetes Deployment manifest
├── service.yaml           # Kubernetes NodePort Service manifest
└── README.md
```

## How to run this locally

1. **Start Minikube**
   ```bash
   minikube start
   ```

2. **Build the Docker image inside Minikube's environment**
   ```bash
   eval $(minikube docker-env)
   docker build -t python-app:latest .
   ```

3. **Apply the Kubernetes manifests**
   ```bash
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   ```

4. **Verify everything is running**
   ```bash
   kubectl get all
   ```

5. **Access the app**

   On macOS (Docker driver), `minikube ip` isn't directly reachable from the host — use the built-in tunnel instead:
   ```bash
   minikube service python-app-service --url
   ```
   Then, in a separate terminal:
   ```bash
   curl -L <printed-url>
   ```

6. **Inspect live traffic with Kubeshark**
   ```bash
   kubeshark tap
   ```
   This opens a dashboard showing real-time requests flowing between pods and services.

## Key concepts learned

- **Cluster vs Node** — a cluster is the overall system; Minikube runs it as a single node containing both control plane and worker components.
- **Pod vs Container** — a Pod is the smallest deployable unit and typically wraps one running container.
- **Deployment vs ReplicaSet** — a Deployment manages the desired state (e.g. "run 2 replicas"), and the ReplicaSet it creates ensures that many pods are always running.
- **Service (NodePort)** — provides a stable way to reach pods, since individual pod IPs change as pods are recreated.

## Troubleshooting notes

**Issue:** `curl http://$(minikube ip):<port>` fails with "Could not connect to server" on macOS.

**Cause:** On macOS, Docker runs inside a lightweight Linux VM (via Docker Desktop). When Minikube uses the Docker driver, the node's IP is only reachable from inside that VM — not directly from the host machine's terminal.

**Fix:** Use `minikube service <service-name> --url` to open a local tunnel, and keep that terminal running while curling from a second terminal. Alternatively, run `minikube tunnel` in a separate terminal to route traffic directly to the NodePort IP.

## Future improvements

- Add Horizontal Pod Autoscaler
- Add liveness/readiness probes
- Deploy to a managed cluster (EKS/GKE) instead of Minikube
- Add CI/CD with GitHub Actions

---

If you found this useful, feel free to ⭐ star the repo.
