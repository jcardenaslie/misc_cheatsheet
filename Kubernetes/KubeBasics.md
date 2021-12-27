
# VERSION
kubectl version -> version

# TERMINOLOGY

## Kubernetes Cluster: 
A collections of nodes + a master to manage them

## Node: 
A virtual machinethat will run our containers

## Pod: 
More or less a running container

## Deployment: 
Monitors a set of pods, make sure they are running and restars them if they crash

Once the application instances are created, a Kubernetes Deployment Controller continuously monitors those instances. If the Node hosting an instance goes down or is deleted, the Deployment controller replaces the instance with an instance on another Node in the cluster. This provides a self-healing mechanism to address machine failure or maintenance.

In a pre-orchestration world, installation scripts would often be used to start applications, but they did not allow recovery from machine failure. By both creating your application instances and keeping them running across Nodes, Kubernetes Deployments provide a fundamentally different approach to application management.

## Service: 
Provides an easy to remember URL to acces a running container

## Issuer and Cluster Issuers
Clusterissuer vs issuer
An Issuer is scoped to a single namespace, and can only fulfill Certificate resources within its own namespace. This is useful in a multi-tenant environment where multiple teams or independent parties operate within a single cluster. On the other hand, a ClusterIssuer is a cluster wide version of an Issuer.

# Config Files: About
Tells Kubernetes about the different Depployments, Pods, and Services that we want to create
Written in YAML syntax
Always store these files with our project source code - they are documentation!
We can create Objects without config files - **do not do this**.  Config files provide a precise definition of what your cluster is running.
- Kubernetes docs will tell you to run direct commands to create objects - only do this for testing purposes
- Blog posts will tell you to run direct commands to create objects - close the blog post!