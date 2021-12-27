
# Create Pod

## Example
1. kubectl apply -f posts.yaml

# Running Pods
kubectl get pods

# Other Commands
```shell
kubectl get pods                        # Print out information about all of the running pods
kubectl exec -it {pod_name} {command}  # Execute the given command in a running pod, command: sh
kubectl logs {pod_name}                 # Print out logs from the given pod 
kubectl delete pod {pod_name}           # Deletes the given pod
kubectl apply -f {config_file_name}     # Tells kubernetes to process the config
kubectl describe pod {pod_name}         # Print out some information about the running pod 
```

```sh
kubectl get pods -l "app=nginx" -o yaml
```

# Pod Example.

```yaml
apiVersion: v1                      # K is extensible, we can add in our own custom objects, This specifies the set objects we want K to look at
kind: Pod                           # The type of object we want to create
metadata:                           # Config options for the object to create
  name: posts                       # When the pod is created, give it a name of 'posts'
spec:                               # The exact attrites we want to apply to the object we are about to create
  containers:                       # We can create many containers in a single pod
    - name: posts                   # Make a container with a name of 'posts'
      image: joaquin/posts:0.0.1    # the exact image we want to use
```