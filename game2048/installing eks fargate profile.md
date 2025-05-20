## Please follow the prerequisites documentation before this.

## Provision the EKS Cluster with fargate as compute engine

```
eksctl create cluster --name jhon-cluster --region ap-southeast-1 --fargate
```

## Configure your local kubectl to connect to your EKS (Elastic Kubernetes Service) cluster on AWS. Explanation: Without this, running kubectl get pods, kubectl apply, etc., won’t work, because kubectl won’t know what cluster you're trying to interact with.

```
aws eks update-kubeconfig --name jhon-cluster --region ap-southeast-1
```

## Create fargate profile. Explanation: Fargate profile will tell EKS that all pods inside that namespace should run on Fargate containers.

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
