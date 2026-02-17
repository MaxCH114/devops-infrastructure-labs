# EKS Nodegroups Management (On-Demand + Spot)

This module demonstrates how to manage Amazon EKS nodegroups using `eksctl`, including adding a second nodegroup with a **mixed capacity strategy** (On-Demand + Spot instances).

---

## Prerequisites

- An existing EKS cluster running:
  - Cluster name: `eks-platform`
  - Region: `us-east-1`
- AWS CLI configured (`aws configure`)
- `eksctl` installed
- `kubectl` installed
- An existing EC2 Key Pair in AWS:
  - `mydemokeypair`

---

## Files

- `eksctl-nodegroups.yaml`  
  Contains the EKS cluster metadata and both nodegroups definitions:
  - `ng-general` (On-Demand)
  - `ng-mixed` (Mixed On-Demand + Spot)

---

## Add a Mixed Nodegroup (On-Demand + Spot)

Create only the nodegroup `ng-mixed`:

```bash
eksctl create nodegroup \
  --config-file=eksctl-nodegroups.yaml \
  --include='ng-mixed'


#### Scale a Nodegroup

# Scale ng-general to 5 nodes:

eksctl scale nodegroup \
  --cluster=eks-platform \
  --name=ng-general \
  --nodes=5


 ### Delete a Nodegroup

## Delete ng-mixed using the cluster name:

eksctl delete nodegroup \
  --cluster=eks-platform \
  --name=ng-mixed


  Or delete it using the config file:

eksctl delete nodegroup \
  --config-file=eksctl-nodegroups.yaml \
  --include=ng-mixed \
  --approve

  
### Verify nodegroups:

eksctl get nodegroup --cluster=eks-platform


### Verify nodes in Kubernetes:

kubectl get nodes -o wide


Notes

Spot instances can be interrupted by AWS at any time.

A mixed nodegroup improves cost efficiency while keeping baseline capacity stable.

Do NOT commit private keys (*.pem) or kubeconfig files.