# Core Concepts (13%)

### Create a namespace called 'mynamespace' and a pod with image nginx called nginx on this namespace
```
kubectl create namespace mynamespace
kubectl run nginx --image=nginx --restart=Never -n mynamespace
```
### Create the pod that was just described using YAML
```
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml | kubectl create -n mynamespace -f -
```
### Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output
```
kubectl run busybox --image=busybox --command --restart=Never -- env
kubectl logs busybox

```

### Create a busybox pod (using YAML) that runs the command "env". Run it and see the output
```
kubectl run busybox --image=busybox --restart=Never --dry-run=client -o yaml --command -- env > envpod.yaml
# see it
cat envpod.yaml
```
### Get the YAML for a new namespace called 'myns' without creating it
```
kubectl create namespace myns --dry-run=client -o yaml
```
### Get the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it
```
controlplane $ kubectl create quota myrq   --hard=cpu=1000m,memory=1G,pod=2 --dry-run=client -o yaml
```
### Get pods on all namespaces
```
kubectl get pods --all-namespace
```
### Create a pod with image nginx called nginx and expose traffic on port 80
```
kubectl run nginx --image=nginx --port=80

```

### Change pod's image to nginx:1.7.1. Observe that the container will be restarted as soon as the image gets pulled
```

```
### Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'
### Get pod's YAML
### Get information about the pod, including details about potential issues (e.g. pod hasn't started)
### Get pod logs
```
kubectl logs nginx
```

### If pod crashed and restarted, get logs about the previous instance
### Execute a simple shell on the nginx pod
### Create a busybox pod that echoes 'hello world' and then exits
```
kubectl run busybox --image=busybox -it --rm --restart=Never -- /bin/sh -c 'echo hello world'

```
### Do the same, but have the pod deleted automatically when it's completed
```
kubectl run busybox --image=busybox -it --rm --restart=Never -- /bin/sh -c 'echo hello world'

```
### Create an nginx pod and set an env value as 'var1=val1'. Check the env value existence within the pod
```
kubectl run nginx --restart=Never --image=nginx --env=var1=val1 -it --rm -- env

```
