# 2048 Application


## Deploy the manifest file like deployment, service and Ingress


```
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

## Verify that the deployments are running
```
kubectl get pods -n game-2048
```
```
kubectl get svc -n game-2048
```
```
kubectl get ingress -n game-2048
```

![Screenshot 2023-08-03 at 7 57 15 PM](https://github.com/iam-veeramalla/aws-devops-zero-to-hero/assets/43399466/93b06a9f-67f9-404f-b0ad-18e3095b7353)
