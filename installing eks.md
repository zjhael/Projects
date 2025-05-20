# Install EKS

Please follow the prerequisites doc before this.

## Install using Fargate and change the cluster name and your region

```
eksctl create cluster --name jhon-cluster --region ap-southeast-1 --fargate
```

## Delete the cluster

```
eksctl delete cluster --name jhon-cluster --region ap-southeast-1
```
