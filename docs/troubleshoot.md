# Troubleshooting Guide

## Overview

This document records the issues encountered during the implementation of the StreamingApp DevOps project and the steps taken to resolve them.

---

# Issue 1: Jenkins Not Accessible

## Problem

Jenkins was not accessible after the EC2 instance was restarted.

## Root Cause

The Jenkins Docker container was stopped because the EC2 instance had been stopped.

## Resolution

Started the Jenkins container:

```bash
docker start jenkins
```

Verified Jenkins using:

```text
http://<EC2-Public-IP>:8080
```

---

# Issue 2: GitHub Webhook Failed

## Problem

GitHub webhook deliveries failed with:

```
Failed to connect to host
```

## Root Cause

The EC2 public IP changed after restarting the instance. GitHub was still sending webhook requests to the old IP address.

## Resolution

* Identified the new EC2 public IP.
* Updated the GitHub webhook URL.
* Redelivered the webhook.
* Verified HTTP Response 200.

---

# Issue 3: Jenkins Pipeline Not Triggering

## Problem

GitHub push did not trigger Jenkins automatically.

## Root Cause

Webhook configuration was pointing to the outdated Jenkins endpoint.

## Resolution

Updated the webhook URL and verified successful automatic pipeline execution.

---

# Issue 4: Accidental Git Commit

## Problem

Executed:

```bash
git add .
```

This staged AWS CLI installation files and other unnecessary project files.

## Resolution

Reset the commit, updated the `.gitignore` file, staged only the required files, and created a clean commit.

---

# Issue 5: Helm Templates Missing

## Problem

`helm template` rendered only a subset of Kubernetes resources.

## Root Cause

Some Helm template files had not yet been created in the `templates/` directory.

## Resolution

Created template files for all required microservices and validated the chart using:

```bash
helm lint
helm template
```

---

# Issue 6: EKS Cluster Deletion

## Problem

After initiating cluster deletion, there was uncertainty whether all resources had been removed.

## Resolution

Verified cleanup using AWS CLI:

```bash
aws eks list-clusters
```

Confirmed:

* EKS cluster deleted
* No remaining Load Balancers
* Node groups removed

This prevented unnecessary AWS charges.

---

# Lessons Learned

* Use Elastic IP for Jenkins to avoid webhook failures after EC2 restarts.
* Validate Helm charts before deployment.
* Use `.gitignore` to prevent accidental commits.
* Verify AWS resources after cleanup to minimize costs.
* Document troubleshooting steps for future reference.

---

# Best Practices Followed

* Automated CI/CD using Jenkins.
* Container image management using Amazon ECR.
* Kubernetes deployments managed through Helm.
* Monitoring using Amazon CloudWatch.
* Alerting using Amazon SNS.
* Cost optimization by deleting EKS resources after use.

