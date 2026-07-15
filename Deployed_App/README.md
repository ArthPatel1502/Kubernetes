# Kubernetes with Minikube 🚀

> A beginner-friendly repository documenting my hands-on journey of learning Kubernetes using Minikube on macOS.

---

## 📖 About the Project

This repository contains my notes, commands, explanations, architecture diagrams, and practical exercises while learning Kubernetes using **Minikube**.

The goal of this project is to understand the fundamentals of Kubernetes before moving on to cloud platforms such as AWS EKS.

---

# 📚 Topics Covered

- Introduction to Kubernetes
- Kubernetes Architecture
- Control Plane Components
- Worker Nodes
- Pods
- Kubernetes Cluster
- Minikube
- kubectl
- Minikube SSH
- Kubernetes Commands
- Local Kubernetes Setup
- Basic Troubleshooting

---

# 🛠 Technologies Used

- macOS
- Docker Desktop
- Kubernetes
- Minikube
- kubectl
- Homebrew

---

# 📦 Installation

## Install Docker Desktop

```bash
brew install --cask docker
```

Start Docker Desktop.

Verify installation

```bash
docker version
```

---

## Install kubectl

```bash
brew install kubectl
```

Verify

```bash
kubectl version --client
```

---

## Install Minikube

```bash
brew install minikube
```

Verify

```bash
minikube version
```

---

## Start Kubernetes Cluster

```bash
minikube start --driver=docker
```

Verify

```bash
kubectl get nodes
```

Expected Output

```
NAME       STATUS   ROLES           AGE
minikube   Ready    control-plane   2m
```

---

# Kubernetes Architecture

```
                    Kubernetes Cluster

        +---------------------------------------+
        |           Control Plane               |
        |---------------------------------------|
        | API Server                            |
        | Scheduler                             |
        | Controller Manager                    |
        | etcd                                  |
        +----------------+----------------------+
                         |
        -----------------------------------------
        |                                       |
+---------------------+              +---------------------+
|    Worker Node      |              |    Worker Node      |
|---------------------|              |---------------------|
| kubelet             |              | kubelet             |
| kube-proxy          |              | kube-proxy          |
| Container Runtime   |              | Container Runtime   |
| Pods                |              | Pods                |
+---------------------+              +---------------------+
```

---

# Minikube Architecture

```
                 macOS

                    │

        Docker Desktop

                    │

             Minikube Cluster

                    │

      +-----------------------------+
      |      Control Plane          |
      |-----------------------------|
      | API Server                  |
      | Scheduler                   |
      | Controller Manager          |
      | etcd                        |
      +-----------------------------+

                    │

      +-----------------------------+
      |        Worker Node          |
      |-----------------------------|
      | kubelet                     |
      | kube-proxy                  |
      | containerd                  |
      | Pods                        |
      +-----------------------------+
```

---

# Important Concepts Learned

## Kubernetes

Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and monitor containerized applications.

---

## Cluster

A Kubernetes Cluster is a collection of machines (nodes) working together.

A cluster contains

- Control Plane
- Worker Nodes

---

## Control Plane

The Control Plane manages the entire cluster.

Components include

- API Server
- Scheduler
- Controller Manager
- etcd

Responsibilities

- Scheduling Pods
- Monitoring Cluster
- Managing Desired State
- Exposing Kubernetes API

---

## Worker Node

Worker Nodes run the application workloads.

Each Worker Node contains

- kubelet
- kube-proxy
- Container Runtime
- Pods

---

## Pod

A Pod is the smallest deployable unit in Kubernetes.

A Pod contains one or more containers.

Example

```
Pod

├── nginx Container
```

---

## Minikube

Minikube is a tool that creates a local Kubernetes cluster for development and learning.

It is ideal for beginners because it allows running Kubernetes on a single machine.

---

## kubectl

kubectl is the command-line interface used to communicate with a Kubernetes cluster.

Examples

```bash
kubectl get nodes

kubectl get pods

kubectl describe pod

kubectl logs

kubectl exec
```

---

## minikube ssh

Used to access the Kubernetes node created by Minikube.

```bash
minikube ssh
```

This allows inspection of the underlying Linux node.

---

# Common Commands

## Cluster Commands

```bash
minikube start

minikube stop

minikube delete

minikube status
```

---

## Node Commands

```bash
kubectl get nodes

kubectl describe node
```

---

## Pod Commands

```bash
kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl exec -it <pod-name> -- bash

kubectl delete pod <pod-name>
```

---

## Cluster Information

```bash
kubectl cluster-info
```

---

# Learning Outcomes

During this project I learned:

- Installing Docker Desktop
- Installing kubectl
- Installing Minikube
- Creating a local Kubernetes cluster
- Understanding Kubernetes architecture
- Understanding Control Plane
- Understanding Worker Nodes
- Understanding Pods
- Using kubectl
- Using minikube ssh
- Managing Kubernetes clusters locally
- Basic Kubernetes troubleshooting

---

# Repository Structure

```
kubernetes-minikube-basics

├── README.md
├── docs
├── screenshots
├── yaml
└── diagrams
```

---

# Future Topics

- ReplicaSets
- Deployments
- Services
- Namespaces
- Labels & Selectors
- ConfigMaps
- Secrets
- Volumes
- Persistent Volumes
- Ingress
- Helm
- StatefulSets

---

# References

- Kubernetes Official Documentation
- Minikube Documentation
- kubectl Documentation

---

# Author

**Arth Patel**

B.Tech Information Technology

Learning DevOps | Docker | Kubernetes | AWS | Terraform | Jenkins
