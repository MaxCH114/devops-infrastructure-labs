# Exposing Applications on EKS

This module demonstrates deploying a scalable Nginx application and exposing it to the internet using a Kubernetes **LoadBalancer Service**, which automatically provisions an AWS Elastic Load Balancer (ELB).

---

## Deploying the Application

### 1. Apply the Deployment
Deploy the 3-replica Nginx app:
```bash
kubectl apply -f deployment.yaml
```

Verify that the pods are running and distributed across your nodes:
```bash
kubectl get pods -o wide
```

### 2. Apply the Service
Create the service to expose the deployment:
```bash
kubectl apply -f service.yaml
```

### 3. Retrieve the Load Balancer DNS
Kubernetes will ask AWS to provision a Classic Load Balancer (CLB). Run the following to check its status:
```bash
kubectl get svc nginx-service
```

Look at the `EXTERNAL-IP` field. It should display a hostname ending in `.amazonaws.com`.
> [!NOTE]
> It may take 2-3 minutes for AWS to provision the ELB and for the DNS to propagate. You can test accessing it in your browser once it is ready.

---

## Cleaning Up

Delete the service and deployment:
```bash
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
```
> [!IMPORTANT]
> Always delete the `LoadBalancer` service before deleting the cluster to ensure that the AWS Elastic Load Balancer resource is cleanly removed from your AWS account and doesn't get orphaned.
