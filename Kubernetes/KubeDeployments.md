# Deployments

# Deployments Commands
```shell
kubectl get deployments
kubectl describe deployment {deployment_name}
kubectl apply -f {config_file}
kubectl delete deployment {depl_name}
kubectl rollout restart deployment {depl_name}
```

# Deployments Example File

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: posts-depl
spec:
  replicas: 1
  selector:
    matchLabels:
      app: posts
  template:
    metadata:
      labels:
        app: posts
    spec:
      containers:
      - name: posts
        image: joaquin/posts:0.0.1
```
