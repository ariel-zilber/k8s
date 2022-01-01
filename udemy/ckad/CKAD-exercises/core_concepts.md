# Core Concepts (13%)

### Create a namespace called 'mynamespace' and a pod with image nginx called nginx on this namespace
### Create the pod that was just described using YAML
### Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output
### Create a busybox pod (using YAML) that runs the command "env". Run it and see the output
### Get the YAML for a new namespace called 'myns' without creating it
### Get the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it
### Get pods on all namespaces
### Create a pod with image nginx called nginx and expose traffic on port 80
### Change pod's image to nginx:1.7.1. Observe that the container will be restarted as soon as the image gets pulled
### Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'
### Get pod's YAML
### Get information about the pod, including details about potential issues (e.g. pod hasn't started)
### Get pod logs
### If pod crashed and restarted, get logs about the previous instance
### Execute a simple shell on the nginx pod
### Create a busybox pod that echoes 'hello world' and then exits
### Do the same, but have the pod deleted automatically when it's completed
### Create an nginx pod and set an env value as 'var1=val1'. Check the env value existence within the pod
