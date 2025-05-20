# Install EKS

Please follow the prerequisites doc before this.

## Provision the EKS Cluster with fargate as compute engine and create a fargae profile

```
eksctl create cluster --name jhon-cluster --region ap-southeast-1 --fargate
```

```
eksctl create fargateprofile \
    --cluster jhon-cluster \
    --region ap-southeast-1 \
    --name alb-sample-app \
    --namespace game-2048
```

## Delete the cluster (Do this after you finish this project)

```
eksctl delete cluster --name jhon-cluster --region ap-southeast-1
```
