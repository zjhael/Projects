# EKS Fargate Cluster with AWS Load Balancer Controller + 2048 Sample App

This project demonstrates how to provision an **Amazon EKS Cluster on Fargate**, deploy the **AWS Load Balancer Controller**, and expose a sample **2048 game** via an **Ingress**.

---

## Stack Used

- `eksctl`
- `kubectl`
- `AWS CLI`
- `Helm`
- `EKS on Fargate`
- `AWS Load Balancer Controller`
- Sample app: `2048`

---

## Prerequisites

- AWS CLI configured
- `eksctl`, `kubectl`, `helm` installed
- IAM user with sufficient permissions (EKS, IAM, VPC, etc.)
- Default VPC or custom VPC (update VPC ID accordingly)

---

### 1. Create EKS Fargate Cluster

```
eksctl create cluster \
  --name jhon-cluster \
  --region ap-southeast-1 \
  --fargate
```

## 2. Update kubeconfig
```
aws eks update-kubeconfig \
  --name jhon-cluster \
  --region ap-southeast-1
```

## 3. Create Fargate Profile for Namespace
```
eksctl create fargateprofile \
  --cluster jhon-cluster \
  --region ap-southeast-1 \
  --name alb-sample-app \
  --namespace game-2048
```
## 4. Deploy Sample 2048 App
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

## 5. Verify Deployments
```
kubectl get pods -n game-2048
```
```
kubectl get svc -n game-2048
```
```
kubectl get ingress -n game-2048
```

##Configure IAM & OIDC for Load Balancer Controller
## 6. Associate OIDC Provider
```
eksctl utils associate-iam-oidc-provider \
  --cluster jhon-cluster \
  --approve
```

## 7. Create IAM Policy for Controller
```
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
```
```
aws iam create-policy \
  --policy-name AWSLoadBalancerControllerIAMPolicy \
  --policy-document file://iam_policy.json
```

## 8. Create IAM Service Account
```
eksctl create iamserviceaccount \
  --cluster=jhon-cluster \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::<YOUR_AWS_ACCOUNT_ID>:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve
```

```Replace <YOUR_AWS_ACCOUNT_ID> with your actual account ID.```

```Install AWS Load Balancer Controller via Helm```
```
helm repo add eks https://aws.github.io/eks-charts
helm repo update eks
```
```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system \
  --set clusterName=jhon-cluster \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=ap-southeast-1 \
  --set vpcId=vpc-xxxxxxxxxxxxxxxxx
```

```Replace vpcId with your actual VPC ID.```

```Verify Deployment```
```
kubectl get deploy -n kube-system aws-load-balancer-controller
```
```
kubectl get deploy -n kube-system
```

```You should see your controller deployed and managing Ingress resources.```

```Access the App```
```
kubectl get ingress -n game-2048
```

```Copy the ADDRESS and open it in a browser to play the 2048 game!```

```Cleanup```
eksctl delete cluster --name demo-cluster-1 --region ap-southeast-1

