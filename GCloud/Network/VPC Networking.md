View the routes
Routes tell VM instances and the VPC network how to send traffic from an instance to a destination, either inside the network or outside Google Cloud. Each VPC network comes with some default routes to route traffic among its subnets and send traffic from eligible instances to the internet.

In the left pane, click Routes. Notice that there is a route for each subnet and one for the Default internet gateway (0.0.0.0/0). These routes are managed for you, but you can create custom static routes to direct some packets to specific destinations. For example, you can create a route that sends all outbound traffic to an instance configured as a NAT gateway.


```sh
gcloud compute networks create privatenet --subnet-mode=custom
```

```sh
gcloud compute networks subnets create privatesubnet-us --network=privatenet --region=us-central1 --range=172.16.0.0/24
```

```sh
gcloud compute networks subnets create privatesubnet-eu --network=privatenet --region=europe-central2 --range=172.20.0.0/20
```

```sh
gcloud compute networks list
```

```sh
gcloud compute networks subnets list --sort-by=NETWORK
```

```sh
gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0
```

´´´
gcloud compute firewall-rules list --sort-by=NETWORK
´´´