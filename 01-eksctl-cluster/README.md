# AWS EKS Cluster Provisioning (eksctl)

This repository contains an `eksctl` configuration file used to provision an Amazon EKS cluster in **us-east-1** with a scalable node group.

## Overview
The cluster is created using Infrastructure-as-Code (IaC) principles with `eksctl`.

Key features:
- Amazon EKS cluster (Kubernetes)
- Node group with autoscaling settings
- SSH access enabled using an existing EC2 key pair
- Clean repository structure with secrets excluded via `.gitignore`

## Requirements
Before running this project, make sure you have:

- AWS CLI configured (`aws configure`)
- `eksctl` installed
- `kubectl` installed
- An existing EC2 Key Pair in AWS (example: `mydemokeypair`)
- IAM permissions to create EKS resources

## Files
- `eksctl-cluster.yaml` → Main cluster configuration
- `.gitignore` → Prevents secrets/keys from being committed

## Deploy the Cluster
Run:

```bash
eksctl create cluster -f eksctl-cluster.yaml
