# Deployment Guide

## Overview

This document describes the deployment process of the StreamingApp application on Amazon Web Services using Docker, Jenkins, Amazon ECR, Amazon EKS, Kubernetes and Helm.

---

# Prerequisites

The following tools and services must be available before deployment:

* AWS Account
* AWS CLI
* Docker
* Git
* Jenkins
* kubectl
* eksctl
* Helm
* GitHub Repository
* Amazon ECR Repositories
* Amazon EKS Cluster

---

# Deployment Workflow

## 1. Clone Repository

```bash
git clone https://github.com/Vilas-Ingle/StreamingApp.git
cd StreamingApp
```

---

## 2. Build Docker Images

Each microservice is built as an independent Docker image.

Example:

```bash
docker build -t streamingapp-auth ./backend/authService
```

Repeat for:

* streamingapp-auth
* streamingapp-streaming
* streamingapp-admin
* streamingapp-chat
* streamingapp-frontend

---

## 3. Push Images to Amazon ECR

Authenticate Docker:

```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com
```

Tag image:

```bash
docker tag streamingapp-auth:latest <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/streamingapp-auth:latest
```

Push image:

```bash
docker push <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/streamingapp-auth:latest
```

Repeat for all microservices.

---

## 4. Configure Jenkins

* Create a Pipeline Job.
* Connect GitHub Repository.
* Configure GitHub Webhook.
* Store AWS credentials securely in Jenkins Credentials.
* Configure the Jenkins pipeline using the Jenkinsfile.

---

## 5. Create Amazon EKS Cluster

Example:

```bash
eksctl create cluster \
--name streamingapp-cluster \
--region ap-south-1 \
--nodegroup-name worker-nodes \
--node-type t3.medium \
--nodes 2
```

---

## 6. Verify Cluster

```bash
kubectl get nodes
```

Expected:

* Worker nodes should be in Ready state.

---

## 7. Deploy using Helm

Validate chart:

```bash
helm lint helm/streamingapp
```

Render templates:

```bash
helm template streamingapp helm/streamingapp
```

Install application:

```bash
helm upgrade --install streamingapp helm/streamingapp -n streamingapp
```

---

## 8. Verify Deployment

Check Pods:

```bash
kubectl get pods
```

Check Services:

```bash
kubectl get svc
```

Check Deployments:

```bash
kubectl get deployments
``` 

check Helm Releases:
```bash
helm list -A

Verify all Kubernetes resources:

```bash
kubectl get all -n streamingapp
``````

---

## 9. Monitoring

Amazon CloudWatch monitors the infrastructure.

Configured alarm:

* EC2 CPU Utilization
* Threshold: 70%

Notifications are delivered through Amazon SNS email subscriptions.

---

## 10. Cleanup

Delete Helm release:

```bash
helm uninstall streamingapp
```

Delete EKS cluster:

```bash
eksctl delete cluster --name streamingapp-cluster --region ap-south-1
```

---

# Deployment Summary

The deployment pipeline performs the following sequence:

1. Developer pushes code to GitHub.
2. GitHub triggers Jenkins through Webhooks.
3. Jenkins builds Docker images.
4. Images are pushed to Amazon ECR.
5. Helm deploys the application to Amazon EKS.
6. Kubernetes manages the application.
7. CloudWatch monitors the infrastructure.
8. Amazon SNS sends alert notifications.

