# Kubernetes Configuration Management (ConfigMaps & Secrets)

This module demonstrates how to decouple application configuration from container images using **ConfigMaps** (for non-sensitive data) and **Secrets** (for sensitive credentials like passwords or API keys).

---

## What We're Deploying
1. **ConfigMap (`configmap.yaml`)**: Stores simple key-value pairs (like `APP_COLOR`) and a full properties file (`app.properties`).
2. **Secret (`secret.yaml`)**: Stores base64 encoded strings (like `DB_PASSWORD`).
3. **Deployment (`deployment.yaml`)**: Consumes these configs in two ways:
   - **Environment Variables**: Injected directly into the container process.
   - **Volume Mounts**: Injected as files inside the container's filesystem under `/etc/config/app.properties`.

---

## Step-by-Step Guide

### 1. Apply the ConfigMap and Secret
```bash
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
```

Verify they have been successfully created:
```bash
kubectl get configmap app-config
kubectl get secret app-secret
```

### 2. Deploy the Application
```bash
kubectl apply -f deployment.yaml
```

Wait for the pod to reach a `Running` state:
```bash
kubectl get pods
```

### 3. Verify Environment Variables
Run a command inside the running pod to check the injected environment variables:
```bash
# Get the pod's name
POD_NAME=$(kubectl get pods -l app=config-demo -o jsonpath="{.items[0].metadata.name}")

# Print the environment variables
kubectl exec -it $POD_NAME -- printenv | grep -E "APP_|DB_|API_"
```
You should see output similar to:
```text
APP_COLOR=blue
APP_MODE=production
DB_PASSWORD=supersecret123
API_KEY=secretkeyxyz
```

### 4. Verify Volume Mounts
Check that the `app.properties` file was correctly mounted as a file:
```bash
kubectl exec -it $POD_NAME -- cat /etc/config/app.properties
```
You should see:
```text
database.connection.timeout=30
logging.level=INFO
feature.flag.new_ui=true
```

---

## Cleaning Up
```bash
kubectl delete -f deployment.yaml
kubectl delete -f secret.yaml
kubectl delete -f configmap.yaml
```
