# Kubernetes Deployment and ReplicaSet

## Overview

In this project, I learned about Kubernetes Deployments and ReplicaSets.

Before understanding Deployment, we need to understand the difference between:

* Container
* Pod
* Deployment

## Container vs Pod vs Deployment

### Container

A container is created using container platforms like Docker.

It contains:

* Application
* Dependencies
* Runtime environment

Example:

```
Docker Container
        |
        |
   Application
```

### Pod

A Pod is the smallest deployable unit in Kubernetes.

Instead of directly running containers, Kubernetes runs containers inside Pods.

A Pod can contain:

* One container
* Multiple containers

Example:

```
Pod

 |
 |
 +---------+
 |         |
Container Container
```

Multiple containers inside a pod can share:

* Network
* Storage

---

# Why Do We Need Deployment?

Creating Pods directly is not recommended for production.

If we create a Pod manually:

```
User

 |

Pod

 |

Container
```

Problems:

* No automatic healing
* No scaling
* Difficult to manage multiple replicas

Kubernetes Deployment provides:

* Auto Healing
* Auto Scaling
* High Availability
* Zero Downtime Deployment

---

# Kubernetes Deployment Architecture

```
              Deployment

                   |

                   |

             ReplicaSet

                   |

        -------------------

        |        |        |

        v        v        v

      Pod-1    Pod-2    Pod-3
```

The flow is:

```
Deployment
      |
      |
ReplicaSet
      |
      |
Pods
```

Deployment creates and manages ReplicaSets.

ReplicaSets create and maintain Pods.

---

# What is ReplicaSet?

ReplicaSet is a Kubernetes controller that maintains the desired number of Pods.

Example:

Deployment specifies:

```
replicas: 3
```

ReplicaSet ensures:

```
Pod-1 Running

Pod-2 Running

Pod-3 Running
```

If any Pod gets deleted:

Before:

```
Pod-1
Pod-2
Pod-3
```

Delete Pod-2:

```
Pod-1
Pod-3
```

ReplicaSet automatically creates a new Pod:

```
Pod-1
Pod-3
Pod-4
```

This behavior is called:

```
Auto Healing
```

---

# Kubernetes Controllers

A controller ensures that the actual state of the cluster matches the desired state defined in YAML.

Example:

Desired State:

```
replicas: 3
```

Actual State:

```
2 Pods running
```

Controller takes action and creates another Pod.

---

# Deployment Features

## 1. Auto Healing

If a Pod fails or is deleted:

```
ReplicaSet
     |
     |
Creates New Pod
```

The application continues running.

---

## 2. Auto Scaling

Deployment allows us to increase or decrease the number of Pods.

Example:

Before:

```
replicas: 1
```

After:

```
replicas: 3
```

Kubernetes creates additional Pods.

---

## 3. Load Balancing

Multiple Pods allow applications to handle multiple users.

Example:

```
        Users

          |

          |

   ----------------

   |       |       |

 Pod-1  Pod-2  Pod-3

```

Traffic can be distributed among multiple Pods.

---

# Deployment YAML

Example:

```yaml
apiVersion: apps/v1

kind: Deployment

metadata:
  name: nginx-deployment


spec:

  replicas: 3


  selector:

    matchLabels:

      app: nginx


  template:

    metadata:

      labels:

        app: nginx


    spec:

      containers:

      - name: nginx

        image: nginx

        ports:

        - containerPort: 80
```

---

# Important YAML Fields

## replicas

Defines how many Pods should run.

Example:

```yaml
replicas: 3
```

---

## selector

Used to identify Pods managed by Deployment.

Example:

```yaml
matchLabels:
  app: nginx
```

---

## template

Defines the Pod configuration.

It contains:

* Labels
* Containers
* Images
* Ports

---

# Creating Deployment

Apply Deployment:

```bash
kubectl apply -f deployment.yaml
```

Check Deployment:

```bash
kubectl get deployments
```

Check ReplicaSet:

```bash
kubectl get rs
```

Check Pods:

```bash
kubectl get pods
```

---

# Testing Auto Healing

View running Pods:

```bash
kubectl get pods
```

Delete a Pod:

```bash
kubectl delete pod <pod-name>
```

Watch Pods:

```bash
kubectl get pods -w
```

A new Pod will automatically be created by ReplicaSet.

---

# Scaling Deployment

Change replicas value in deployment.yaml:

Before:

```yaml
replicas: 1
```

After:

```yaml
replicas: 3
```

Apply changes:

```bash
kubectl apply -f deployment.yaml
```

Check Pods:

```bash
kubectl get pods
```

New Pods will be created automatically.

---

# Deployment vs Pod

| Pod               | Deployment            |
| ----------------- | --------------------- |
| Runs containers   | Manages Pods          |
| No auto healing   | Provides auto healing |
| No scaling        | Provides scaling      |
| Manual management | Automated management  |

---

# Deployment vs ReplicaSet

| Deployment                    | ReplicaSet              |
| ----------------------------- | ----------------------- |
| Creates ReplicaSet            | Creates Pods            |
| Manages application lifecycle | Maintains Pod count     |
| Provides scaling              | Maintains replicas      |
| Used in production            | Works behind Deployment |

---

# Zero Downtime Deployment

Deployment helps update applications without stopping the application completely.

Instead of removing all Pods together:

```
Old Pods

   ↓

New Pods Created

   ↓

Traffic continues
```

This helps achieve zero downtime deployment.

---

# Useful Commands

Get all resources:

```bash
kubectl get all
```

Get Pods:

```bash
kubectl get pods
```

Get ReplicaSets:

```bash
kubectl get rs
```

Get Deployments:

```bash
kubectl get deployment
```

Watch resources:

```bash
kubectl get pods -w
```

Delete Deployment:

```bash
kubectl delete deployment <name>
```

---

# Key Learnings

After completing this implementation:

* Learned difference between Container, Pod, and Deployment
* Learned why Deployment is preferred over creating Pods directly
* Understood Deployment → ReplicaSet → Pod architecture
* Learned Kubernetes controllers
* Learned auto healing using ReplicaSets
* Learned scaling using replicas
* Learned zero downtime deployment concept
* Implemented Deployment using YAML
* Used kubectl commands to manage resources

---

# Git Commands

```bash
git add .

git commit -m "Implemented Kubernetes Deployment and ReplicaSet"

git push origin main
```
