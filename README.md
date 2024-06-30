# ArgoCD Setup Guide

This document outlines the steps to set up and configure ArgoCD on a Kubernetes cluster.

## Prerequisites

- Kubernetes cluster (v1.16+)
- `kubectl` CLI tool
- ArgoCD CLI tool

## Kubernetes Setup

### For this demo we required the kubernetes cluster. If you want to understand how to create the eks we have the video for that. Below is the link.

```sh 
https://youtu.be/33PZszzTWAQ?si=it4Cq1vgXLcO9IW7
```

## ArgoCD Setup
### Here are the steps to install ArgoCD and retrieve the admin password:

1. **Create Namespace for ArgoCD:**
    ```sh
    kubectl create namespace argocd
    ```
2. **Install ArgoCD on Kubernetes Cluster:**
```sh
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
3. **Patch Service Type to LoadBalancer:**
```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
4. **Retrieve Admin Password:**
    ```sh
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```
These commands will install ArgoCD into the specified namespace, set up the service as a LoadBalancer, and retrieve the admin password for you to access the ArgoCD UI.
## ArgoCD CLI Setup

1. **Login to ArgoCD:**
    ```sh
    argocd login <argocd-access-url>
    ```
    Retrieve the initial admin password:
    ```sh
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```

2. **Change Admin Password:**
    ```sh
    argocd account update-password
    ```

## Usage

1. **Add a Git Repository:**
    ```sh
    argocd repo add https://github.com/<your-repo>/<your-app>.git
    ```

2. **Create an Application:**
    ```sh
    argocd app create <app-name> \
      --repo https://github.com/<your-repo>/<your-app>.git \
      --path <app-path> \
      --dest-server https://kubernetes.default.svc \
      --dest-namespace <namespace>
    ```

3. **Sync an Application:**
    ```sh
    argocd app sync <app-name>
    ```

## Useful Commands

- List applications:
    ```sh
    argocd app list
    ```
- Get application details:
    ```sh
    argocd app get <app-name>
    ```
- Delete an application:
    ```sh
    argocd app delete <app-name>
    ```

## Access the Web UI

Access the ArgoCD Web UI at `https://localhost:8080`.

For more detailed information, visit the [ArgoCD documentation](https://argo-cd.readthedocs.io/en/stable/).
