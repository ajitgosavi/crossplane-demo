# Installing Crossplane on Kubernetes

This guide provides step-by-step instructions for installing Crossplane in a Kubernetes cluster.

---

## Prerequisites

Before installing Crossplane, ensure the following prerequisites are met:

1. **Kubernetes Cluster**:
   - A running Kubernetes cluster (v1.21 or newer is recommended).
   - Tools like Minikube, Kind, or a cloud-based Kubernetes service (e.g., GKE, EKS, AKS) can be used.

2. **kubectl**:
   - Install `kubectl`, the Kubernetes CLI, to interact with the cluster.
   - Follow the official installation guide: [kubectl Installation](https://kubernetes.io/docs/tasks/tools/).

3. **Helm**:
   - Install Helm, the Kubernetes package manager.
   - Follow the official installation guide: [Helm Installation](https://helm.sh/docs/intro/install/).

4. **Cluster Admin Privileges**:
   - Ensure you have administrative access to the Kubernetes cluster.

---

## Installation

Follow these steps to install Crossplane in your Kubernetes cluster:

### Step 1: Add the Crossplane Helm Repository

```bash
helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
```

### Step 2: Install the Crossplane Helm Chart
Install Crossplane in the `crossplane-system` namespace:
```bash
helm install crossplane --namespace crossplane-system --create-namespace crossplane-stable/crossplane
```

Verify the installation by checking the pods in the `crossplane-system` namespace:
```bash
kubectl get pods -n crossplane-system
```
Expected output (example):
```bash
NAME                                      READY   STATUS    RESTARTS   AGE
crossplane-xxxxxx-xxxxxx                  1/1     Running   0          xxm
crossplane-rbac-manager-xxxxxx-xxxxxx     1/1     Running   0          xxm
```
### Step 3: Configure Crossplane Providers 
Crossplane requires providers to manage external resources. 
1. To install a aws provider, use the following steps:
```bash
kubectl crossplane install provider crossplane/provider-aws:v0.33.0
```
2. Verify the provider installation:
```bash
kubectl get providers
```
### Step 4: Install CRDs for Providers

Providers often include custom resource definitions (CRDs) to manage resources. After installing a provider, ensure its CRDs are installed:
```bash
kubectl apply -f provider-config.yaml
```
### Step 5: Verify Crossplane Setup
To confirm Crossplane is properly configured, ensure the following:
1. All pods in `crossplane-system` are running.
2. Providers are installed and ready.
