
# Kubernetes Dashboard Setup Guide

A clean, step-by-step guide to installing and accessing the Kubernetes Dashboard with a secure Service Account and token-based authentication.

---

## 🚀 Overview

The **Kubernetes Dashboard** is a web UI for managing and monitoring Kubernetes clusters.
This guide includes:

- Installing the Dashboard
- Creating an admin Service Account
- Generating an access token
- Logging into the Dashboard
- Adding your own screenshot

---

## 📌 1. Install Kubernetes Dashboard

Apply the official Dashboard deployment:

bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

---

📌 2. Create Admin Service Account

Create a file named dashboard-admin.yaml with the following content:

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

---

📌 3. Generate Dashboard Login Token

For Kubernetes v1.24+:

kubectl -n kubernetes-dashboard create token dashboard-admin-user


Copy the generated token.

If the create token command is not supported:

List secrets:

kubectl -n kubernetes-dashboard get secret


Find the secret containing the token and run:

kubectl -n kubernetes-dashboard describe secret <secret-name>

---

📌 4. Access the Dashboard

Start the Kubernetes API proxy:

kubectl proxy


Open the Dashboard:

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/


Select Token, paste your token, and click Sign In.

📸 5. Dashboard Screenshot (Add Here)

Add your screenshot under this line:

![Kubernetes Dashboard Screenshot](assets/dashboard.png)


Replace assets/dashboard.png with your image file name.

---

📌 6. Useful Commands

Check if the Service Account has correct permissions:

kubectl auth can-i get pods --as=system:serviceaccount:kubernetes-dashboard:dashboard-admin-user


Check dashboard pods:

kubectl -n kubernetes-dashboard get pods


Delete dashboard (if needed):

kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
