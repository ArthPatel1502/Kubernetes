# 🚀 Kubernetes

> A beginner-friendly repository documenting my Kubernetes learning journey using **AWS EC2**, **Kops**, **kubectl**, and **Docker**.

---

## 📖 About

This repository is my personal learning guide for Kubernetes. It covers the complete process of understanding Kubernetes architecture, setting up a Kubernetes cluster on AWS using Kops, managing clusters with kubectl, and deploying applications using Kubernetes YAML manifests.

The goal of this repository is not only to document commands, but also to explain **why each component exists**, **how Kubernetes works internally**, and **how different tools interact with each other**.

---

# 🎯 Learning Objectives

After completing this repository, you should understand:

* What Kubernetes is
* Why Kubernetes is needed
* Kubernetes Architecture
* Control Plane Components
* Worker Nodes
* Pods
* Deployments
* ReplicaSets
* Services
* Namespaces
* ConfigMaps
* Secrets
* Persistent Volumes
* Ingress
* Kubernetes Networking
* kubectl
* Kops
* Cluster Creation on AWS
* Troubleshooting Common Kubernetes Issues

---

# 🏗️ Kubernetes Architecture

```
                     Internet
                         │
                         ▼
                 AWS Load Balancer
                         │
                         ▼
                  Kubernetes Cluster
                         │
         ┌───────────────┴───────────────┐
         │                               │
         ▼                               ▼
  Control Plane                    Worker Node
         │                               │
 ┌─────────────────┐              ┌──────────────┐
 │ API Server      │              │ kubelet      │
 │ Scheduler       │              │ kube-proxy   │
 │ Controller      │              │ Container    │
 │ etcd            │              │ Runtime      │
 └─────────────────┘              └──────────────┘
                                          │
                                          ▼
                                    Pods & Containers
```

---

# 📚 What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform.

It automates:

* Application deployment
* Scaling
* Load balancing
* Self-healing
* Rolling updates
* Service discovery

Instead of manually managing Docker containers across multiple servers, Kubernetes manages them automatically.

---

# 🤔 Why Do We Need Kubernetes?

Imagine running hundreds of Docker containers.

Without Kubernetes:

* Restart crashed containers manually
* Scale applications manually
* Configure networking manually
* Distribute containers across servers manually

Kubernetes automates all of these tasks.

---

# 🛠️ Docker vs Kubernetes

| Docker                | Kubernetes             |
| --------------------- | ---------------------- |
| Runs containers       | Manages containers     |
| Single machine        | Multiple machines      |
| Manual scaling        | Automatic scaling      |
| No self-healing       | Automatic self-healing |
| Limited orchestration | Full orchestration     |

---

# ☸️ What is a Kubernetes Cluster?

A Kubernetes Cluster is a collection of machines working together to run containerized applications.

A cluster consists of:

* Control Plane
* Worker Nodes

---

# 🧠 Control Plane

The Control Plane is the brain of Kubernetes.

Components:

### API Server

The main entry point for Kubernetes.

Every command sent through `kubectl` first reaches the API Server.

---

### Scheduler

Decides on which worker node a Pod should run.

---

### Controller Manager

Continuously monitors the cluster and ensures the desired state matches the current state.

---

### etcd

A distributed key-value database storing all cluster information.

---

# 💻 Worker Node

Worker nodes actually run your applications.

Components:

* kubelet
* kube-proxy
* Container Runtime
* Pods

---

# 📦 Pod

A Pod is the smallest deployable unit in Kubernetes.

A Pod usually contains one application container.

---

# 📁 Deployment

A Deployment manages Pods.

It provides:

* Scaling
* Rolling updates
* Rollbacks
* High availability

---

# 🔁 ReplicaSet

ReplicaSets ensure that the desired number of Pod replicas are always running.

If a Pod crashes, Kubernetes automatically creates a new one.

---

# 🌐 Services

Services expose Pods to other applications or users.

Types:

* ClusterIP
* NodePort
* LoadBalancer

---

# 📡 kubectl

`kubectl` is the Kubernetes command-line interface.

Examples:

```bash
kubectl get nodes

kubectl get pods

kubectl get deployments

kubectl describe pod nginx

kubectl logs pod-name

kubectl apply -f deployment.yaml
```

---

# 🚀 What is Kops?

Kops is a Kubernetes cluster provisioning tool for AWS.

Instead of manually creating:

* EC2 Instances
* VPC
* IAM Roles
* Security Groups
* Auto Scaling Groups
* Networking

Kops automates the entire cluster creation process.

---

# 🔄 Kops vs kubectl

| Kops                        | kubectl                     |
| --------------------------- | --------------------------- |
| Creates Kubernetes clusters | Manages Kubernetes clusters |
| AWS Infrastructure          | Kubernetes Resources        |
| One-time setup              | Daily usage                 |

---

# 🧩 How Everything Works Together

```
Developer
     │
     ▼
kubectl Commands
     │
     ▼
Kubernetes API Server
     │
     ▼
Scheduler
     │
     ▼
Worker Nodes
     │
     ▼
Pods
     │
     ▼
Containers
```

Cluster creation:

```
AWS
   │
   ▼
Kops
   │
Creates
   │
Kubernetes Cluster
   │
kubectl
   │
Manage Applications
```

---

# 💡 Topics Covered

* Kubernetes Basics
* Kubernetes Architecture
* Kops
* kubectl
* AWS CLI
* Kubernetes Installation
* Pods
* Deployments
* ReplicaSets
* Services
* Namespaces
* ConfigMaps
* Secrets
* Persistent Volumes
* Ingress
* Networking
* YAML Manifests
* Troubleshooting

---

# 📖 References

Official Documentation:

* https://kubernetes.io/docs/
* https://kops.sigs.k8s.io/
* https://aws.amazon.com/

---

# ⭐ Future Improvements

* Deploy Nginx
* Deploy Node.js Application
* Deploy Flask Application
* Helm Charts
* Metrics Server
* Horizontal Pod Autoscaler
* Monitoring with Prometheus & Grafana
* Kubernetes Dashboard
* Amazon EKS
* CI/CD with Jenkins and Kubernetes

---

If you found this repository useful, feel free to ⭐ star the repository and share feedback.
