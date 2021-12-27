Kubernetes on Google Cloud
When you run a GKE cluster, you also gain the benefit of advanced cluster management features that Google Cloud provides. These include:

- Load balancing for Compute Engine instances
- Node pools to designate subsets of nodes within a cluster for additional flexibility
- Automatic scaling of your cluster's node instance count
- Automatic upgrades for your cluster's node software
- Node auto-repair to maintain node health and availability
- Logging and Monitoring with Cloud Monitoring for visibility into your cluster


---

gcloud container clusters create

kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0

kubectl expose deployment hello-server --type=LoadBalancer --port 8080

gcloud container clusters delete


---

export MY_ZONE=us-central1-a

gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2

kubectl version

kubectl create deploy nginx --image=nginx:1.17.10

kubectl get pods

kubectl expose deployment nginx --port 80 --type LoadBalancer

s

kubectl scale deployment nginx --replicas 3