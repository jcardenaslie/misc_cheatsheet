# Type of Services

## Cluster IP:
Sets up an easy-to-remember URL to access a pod. Only exposes pods in the cluster

## Node Port:
Makes a pod accessible from outside the cluster.  Usually only used for dev purposes

## Load Balancer
Makes a pod accessible from outside the cluster.  This is the right way to expose a pod to the outside world

## External Name
Redirects an in-cluster request to a CNAME url.....don't worry about this one....


```sh
kubectl get services

kubectl scale nginx --replicas=3

kubectl autoscale nginx --min=10 --max=15 --cpu=80

kubectl get replicasets

```