# Kubernetes-Dashboard
# Kubernetes Dashboard Setup Guide

A complete, step-by-step guide to installing and accessing the Kubernetes Dashboard with a secure Service Account and token-based authentication.

---

## 🚀 Overview

The **Kubernetes Dashboard** provides a web-based UI to manage and troubleshoot Kubernetes clusters.  
This guide covers:

- Installing the Dashboard  
- Creating an admin-level Service Account  
- Generating a login token  
- Accessing the Dashboard securely  
- Adding a screenshot section  

---

## 📌 1. Install Kubernetes Dashboard

Apply the official Kubernetes Dashboard manifest:

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

This command installs:

Kubernetes Dashboard UI

Dashboard backend components

Metrics scraper

Required RBAC roles & services

📌 2. Create Admin Service Account (Full Cluster Access)

Create dashboard-admin.yaml:
apiVersion: v1
kind: ServiceAccount
metadata:
  name: dashboard-admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: dashboard-admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: dashboard-admin-user
  namespace: kubernetes-dashboard


