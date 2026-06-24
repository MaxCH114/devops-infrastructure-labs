# EKS Nodegroups Management (On-Demand + Spot)

This module demonstrates how to configure and use multiple node groups, specifically leveraging a **mixed capacity strategy** (On-Demand + Spot instances) to optimize infrastructure costs.

---

## Files

* `eksctl-nodegroups.yaml`: Provisioning configuration for `eksctl` creating two nodegroups:
  * `ng-general`: Fully On-Demand `t3.small` nodes.
  * `ng-mixed`: A cost-optimized blend of On-Demand and Spot instances (`t3.small` / `t3a.small`).
* `spot-deployment.yaml`: A deployment manifest configured to only schedule pods on Spot instances.

---

## Step-by-Step Guide

### 1. Provision Cluster with Nodegroups
```bash
eksctl create cluster -f eksctl-nodegroups.yaml
```

### 2. Verify Node Capacity Types
Check which nodes are running on On-Demand vs Spot instances by listing the nodes with their capacity type labels:
```bash
kubectl get nodes -L alpha.eksctl.io/nodegroup-name -L eks.amazonaws.com/capacityType
```

### 3. Deploy Workloads to Spot Instances
Apply the Spot deployment:
```bash
kubectl apply -f spot-deployment.yaml
```

Verify that all pods are running and are scheduled **only** on the nodes identified as `SPOT`:
```bash
kubectl get pods -o wide
```

### 4. Cleanup
Delete the deployment:
```bash
kubectl delete -f spot-deployment.yaml
```
