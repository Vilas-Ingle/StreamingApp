# Project Report

## Project Title

**StreamingApp - Orchestration and Scaling using Kubernetes, Helm and AWS**

---

# Project Objective

The objective of this project is to deploy a microservices-based MERN application on Amazon Web Services using modern DevOps practices. The project demonstrates containerization, Continuous Integration, Kubernetes orchestration, Helm-based deployment, monitoring and cloud-native infrastructure management.

---

# Technologies Used

## Version Control

* Git
* GitHub

## Containerization

* Docker
* Docker Compose

## CI/CD

* Jenkins
* GitHub Webhooks

## Cloud Platform

* Amazon EC2
* Amazon ECR
* Amazon EKS
* Amazon CloudWatch
* Amazon SNS

## Orchestration

* Kubernetes
* Helm

## Database

* MongoDB

---

# Microservices

The application consists of the following services:

* Frontend Service
* Authentication Service
* Streaming Service
* Admin Service
* Chat Service
* MongoDB Database

Each service is containerized and managed independently within Kubernetes.

---

# CI/CD Workflow

1. Developer pushes code to GitHub.
2. GitHub Webhook triggers Jenkins.
3. Jenkins checks out the latest source code.
4. Docker images are built.
5. Images are pushed to Amazon ECR.
6. Helm deploys the application to Amazon EKS.
7. Kubernetes manages the application workloads.
8. CloudWatch monitors infrastructure.
9. Amazon SNS sends alert notifications.

---

# Monitoring and Alerting

Monitoring was implemented using Amazon CloudWatch.

A CPU utilization alarm was configured for the Jenkins EC2 instance.

Amazon SNS was integrated with CloudWatch to send email notifications when the alarm threshold is exceeded.

---

# Challenges Faced

During implementation, several practical issues were encountered and resolved:

* Jenkins unavailable after EC2 restart.
* GitHub Webhook failures caused by EC2 public IP changes.
* Helm template rendering issues.
* Accidental Git staging of unnecessary files.
* Verification of AWS resource cleanup to avoid additional costs.

These issues were documented along with their resolutions in the troubleshooting guide.

---

# Key Learning Outcomes

This project provided hands-on experience with:

* Docker containerization
* Jenkins CI/CD pipeline creation
* GitHub Webhook integration
* Amazon ECR image management
* Amazon EKS cluster deployment
* Kubernetes resource management
* Helm chart development
* CloudWatch monitoring
* Amazon SNS notifications
* AWS cost optimization

---


# Conclusion

The project successfully demonstrates the deployment of a cloud-native microservices application using modern DevOps tools and AWS managed services. It implements an automated CI/CD workflow, Kubernetes orchestration through Helm, infrastructure monitoring with CloudWatch, and notification services using Amazon SNS, following industry-standard DevOps practices.

