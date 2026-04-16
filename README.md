# GitOps-Based Event Streaming Platform on AWS (EKS + Kafka + Argo CD)

## Overview

This project demonstrates a **cloud-native, event-driven architecture** deployed on **AWS EKS** using **GitOps principles**.

It showcases how modern systems can be:

* Scalable
* Resilient
* Fully automated

By integrating:

* **Kubernetes (EKS)** for orchestration
* **Apache Kafka** for event streaming
* **Argo CD** for GitOps deployment
* **GitHub** as the source of truth

---

## Architecture

```
GitHub (source of truth)
        ↓
     Argo CD
        ↓
     EKS Cluster
   ├── Kafka (event broker)
   ├── Producer (event generator)
   └── Consumer (event processor)
```

---

## Tech Stack

* AWS EKS (Kubernetes)
* Argo CD (GitOps)
* Apache Kafka (Event Streaming)
* Docker (Containers)
* Kubernetes (Deployments, Services)
* kubectl / eksctl / Helm

---

## Features Implemented

* ✅ Kubernetes cluster provisioning using `eksctl`
* ✅ GitOps workflow with Argo CD
* ✅ Event-driven architecture using Kafka
* ✅ Producer/Consumer microservices
* ✅ Horizontal scaling of consumers
* ✅ Self-healing using Kubernetes controllers
* ✅ Rolling updates via Git changes
* ✅ Debugging real-world issues (PVC, image pull, resource constraints)

---

## Getting Started

### 1. Create EKS Cluster

```bash
eksctl create cluster \
  --name devops-lab \
  --region ap-south-1 \
  --node-type t3.small \
  --nodes 2
```

---

### 2. Install Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Expose UI:

```bash
kubectl patch svc argocd-server -n argocd \
  -p '{"spec": {"type": "LoadBalancer"}}'
```

---

### 3. Deploy Kafka

```bash
kubectl apply -f kafka-simple.yaml
```
<img width="477" height="182" alt="apply and get pods" src="https://github.com/user-attachments/assets/fb44711c-e368-46e6-96e0-cc51ca3d33b0" />

---

### 4. Deploy Producer & Consumer

```bash
kubectl apply -f producer.yaml
kubectl apply -f consumer.yaml
```

---

### 5. Verify Event Flow

```bash
kubectl logs -f deployment/consumer
```

---

### 6. GitOps Setup

```bash
git init
git add .
git commit -m "initial gitops manifests"
git push origin main
```

Create Argo CD application and sync.

---
<img width="925" height="433" alt="ArgoCD" src="https://github.com/user-attachments/assets/b34c562d-8da8-432f-87c9-116f028677b5" />


## Hands-On Exercises

### Scale Consumers

```bash
kubectl scale deployment consumer --replicas=3
```
<img width="730" height="212" alt="Scaling up" src="https://github.com/user-attachments/assets/c6401039-f6bb-4e67-8350-f38409006076" />

---

### Test Self-Healing

```bash
kubectl delete pod <pod-name>
```

---

### Rolling Update

Change:

```yaml
sleep 5 → sleep 1
```

Commit to Git → Argo CD syncs

---

### Simulate Drift

```bash
kubectl edit deployment producer
```
<img width="941" height="430" alt="ArgoCD before syncing" src="https://github.com/user-attachments/assets/7f128e74-130f-48b1-86a3-482bb900e012" />
<img width="942" height="430" alt="ArgoCD sync" src="https://github.com/user-attachments/assets/75bf8687-c6c1-4671-9b3c-b6d929abec32" />

---

## Interview Insights

### Why Argo CD?

* Git as source of truth
* Pull-based deployment
* Drift detection

---

### Kafka Concepts

* Consumer groups
* Message partitioning
* Offset management

---

### Kubernetes Concepts

* Desired state
* Self-healing
* Scaling

---

### Production Improvements

* Multi-node Kafka cluster
* Persistent storage (EBS)
* Monitoring (Prometheus + Grafana)
* Secure communication (TLS)

---

## Challenges Faced

* Resource constraints on small nodes
* Persistent volume binding issues
* Image pull failures due to deprecated tags
* Need to simplify Kafka deployment

---

## Key Learnings

* GitOps simplifies deployment and management
* Kubernetes ensures system resilience
* Event-driven systems scale efficiently
* Debugging is a critical DevOps skill

---

## Future Improvements

* Add Prometheus & Grafana monitoring
* Implement CI/CD with GitHub Actions
* Deploy Kafka in production-grade setup
* Introduce namespaces and RBAC

---

## Conclusion

This project demonstrates how to build a **modern, scalable, and automated system** using Kubernetes, Kafka, and GitOps.

It reflects real-world DevOps and platform engineering practices.

---

## Author

**Maaham Banu**
Cloud & DevOps Engineer

---

## If you found this useful

Feel free to:

* Star ⭐ the repo
* Connect on LinkedIn
* Share feedback

---
