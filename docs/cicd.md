# CI/CD Pipeline

## Overview

StreamingApp uses Jenkins to implement a Continuous Integration and Continuous Deployment (CI/CD) pipeline. The pipeline automatically builds, packages, and prepares the application for deployment whenever code is pushed to GitHub.

---

# Pipeline Architecture

```
Developer
    │
    ▼
Git Commit
    │
    ▼
GitHub Repository
    │
GitHub Webhook
    │
    ▼
Jenkins Pipeline
    │
    ├── Checkout Source Code
    ├── Build Docker Images
    ├── Push Images to Amazon ECR
    ├── Validate Helm Chart
    └── Deploy to Amazon EKS (Helm)
```

---

# Pipeline Workflow

## Step 1 – Code Commit

A developer pushes code changes to the GitHub repository.

Example:

```bash
git add .
git commit -m "Implemented feature"
git push origin dev
```

---

## Step 2 – GitHub Webhook

GitHub automatically sends an HTTP POST request to Jenkins using the configured webhook endpoint.

```
http://<JENKINS_PUBLIC_IP>:8080/github-webhook/
```

This eliminates the need for Jenkins to poll the repository continuously.

---

## Step 3 – Jenkins Trigger

The webhook triggers the Jenkins Pipeline automatically.

The pipeline begins by checking out the latest source code from GitHub.

---

## Step 4 – Docker Build

Jenkins builds Docker images for each microservice.

Example:

* Auth Service
* Streaming Service
* Admin Service
* Chat Service
* Frontend

---

## Step 5 – Push Images

The Docker images are tagged and pushed to Amazon Elastic Container Registry (ECR).

This ensures Kubernetes always pulls images from a centralized and secure registry.

---

## Step 6 – Helm Validation

Before deployment, Jenkins validates the Helm chart using:

```bash
helm lint
helm template
```

This helps detect template or syntax errors before deployment.

---

## Step 7 – Kubernetes Deployment

The validated Helm chart is deployed to the Amazon EKS cluster.

Helm manages the Kubernetes resources including:

* Deployments
* Services
* ReplicaSets
* Pods

---

## Step 8 – Monitoring

Amazon CloudWatch monitors the deployed infrastructure.

CloudWatch alarms send notifications through Amazon SNS when configured thresholds are exceeded.

---

# Benefits of the Pipeline

* Automated build process
* Faster deployments
* Reduced manual effort
* Consistent deployments
* Easy rollback using Helm
* Centralized container registry
* Continuous monitoring
* Improved reliability

---

# Technologies Used

* Git
* GitHub
* GitHub Webhooks
* Jenkins
* Docker
* Amazon ECR
* Amazon EKS
* Helm
* Kubernetes
* Amazon CloudWatch
* Amazon SNS

