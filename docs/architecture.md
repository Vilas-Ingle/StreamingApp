# StreamingApp Architecture

## Overview

StreamingApp is a cloud-native MERN stack application deployed on Amazon Web Services using Kubernetes and modern DevOps practices.

The application consists of multiple microservices packaged as Docker containers, stored in Amazon ECR, and deployed to Amazon EKS using Helm charts. Continuous Integration is implemented using Jenkins, while CloudWatch and Amazon SNS provide monitoring and alerting.

---

# High-Level Architecture

```
                    Developer
                        │
                        ▼
                  GitHub Repository
                        │
               GitHub Webhook
                        │
                        ▼
                 Jenkins Pipeline
                        │
        ┌───────────────┼────────────────┐
        │                                │
        ▼                                ▼
 Docker Image Build             Pipeline Automation
        │
        ▼
 Amazon Elastic Container Registry (ECR)
        │
        ▼
      Helm Charts
        │
        ▼
 Amazon Elastic Kubernetes Service (EKS)
        │
        ▼
 Kubernetes Cluster
        │
 ┌──────┼───────────────┬──────────────┬──────────────┬─────────────┐
 │      │               │              │              │
 ▼      ▼               ▼              ▼              ▼
Auth  Streaming      Admin         Chat         Frontend
Service Service      Service       Service      Service
        │
        ▼
     MongoDB
        │
        ▼
 CloudWatch Monitoring
        │
        ▼
 Amazon SNS
        │
        ▼
 Email Notifications
```

---

# Components

## GitHub

* Stores the application source code.
* Hosts the Jenkinsfile, Kubernetes manifests and Helm charts.
* Sends webhook notifications to Jenkins whenever code is pushed.

---

## Jenkins

* Receives GitHub webhook events.
* Executes the CI/CD pipeline.
* Builds Docker images.
* Pushes images to Amazon ECR.
* Deploys the application using Helm.

---

## Docker

Each microservice is packaged as an independent Docker image, ensuring consistent deployments across environments.

---

## Amazon ECR

Amazon Elastic Container Registry stores Docker images securely and serves them to the Kubernetes cluster during deployment.

---

## Amazon EKS

Amazon Elastic Kubernetes Service provides the managed Kubernetes control plane used to orchestrate all application workloads.

---

## Helm

Helm packages the Kubernetes manifests into reusable charts, simplifying deployment and version management.

---

## Kubernetes

The Kubernetes cluster manages the complete lifecycle of all StreamingApp microservices, including:

- Deployments
- ReplicaSets
- Pods
- Services
- Self-healing of failed containers
- Rolling updates during deployments

---

## CloudWatch

Amazon CloudWatch monitors infrastructure metrics such as CPU utilization and generates alarms when thresholds are exceeded.

---

## Amazon SNS

Amazon SNS sends email notifications whenever CloudWatch alarms enter the ALARM state.

---

# Microservices

| Service           | Purpose                                |
| ----------------- | -------------------------------------- |
| Auth Service      | User authentication and JWT management |
| Streaming Service | Video streaming APIs                   |
| Admin Service     | Content management                     |
| Chat Service      | Real-time communication                |
| Frontend          | React user interface                   |
| MongoDB           | Persistent data storage                |

---

# Benefits of the Architecture

* Microservice-based design
* Independent deployment of services
* Automated CI/CD pipeline
* Containerized workloads
* Kubernetes orchestration
* Simplified deployments with Helm
* Centralized monitoring
* Automated alert notifications
* Scalable cloud-native infrastructure

