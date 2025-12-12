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
This command installs:

Kubernetes Dashboard UI

Dashboard backend components

Metrics scraper

Required RBAC roles & services

---

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


Apply the configuration:

kubectl apply -f dashboard-admin.yaml
---
📌 3. Generate Dashboard Access Token

For Kubernetes v1.24+:

kubectl -n kubernetes-dashboard create token dashboard-admin-user


Copy the generated JWT token — it will be used to log in.

If your cluster does not support create token:

List secrets:

kubectl -n kubernetes-dashboard get secret


Get the token from the related secret:

kubectl -n kubernetes-dashboard describe secret <secret-name>

📌 4. Access the Dashboard

Start the proxy service:

kubectl proxy


Open the Dashboard in your browser:

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/


At the login screen → choose Token → paste your token → Sign In.

📸 5. Dashboard Screenshot

Add your Kubernetes Dashboard screenshot in this section:

![Kubernetes Dashboard Screenshot](assets/dashboard-screenshot.png)


Replace the file path with the name/location of your screenshot.

📌 6. Verification Commands

Check if the Service Account has the expected access:

kubectl auth can-i get pods --as=system:serviceaccount:kubernetes-dashboard:dashboard-admin-user


Check Dashboard resources:

kubectl -n kubernetes-dashboard get pods


Remove the Dashboard (if needed):

kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml


