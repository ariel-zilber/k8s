### Create 3 pods with names nginx1,nginx2,nginx3. All of them should have the label app=v1
```
kubectl run nginx1 --image=nginx --restart=Never --labels=app=v1
kubectl run nginx2 --image=nginx --restart=Never --labels=app=v1
kubectl run nginx3 --image=nginx --restart=Never --labels=app=v1

```
### Show all labels of the pods
```
kubectl get po --show-labels

```
### Change the labels of pod 'nginx2' to be app=v2
```
kubectl label pod nginx2 app=v2 --overwrite
```
### Get the label 'app' for the pods (show a column with APP labels)
```
 kubectl get pod -l app
```
### Get only the 'app=v2' pods
```
kubectl get po -l app=v2

```
### Remove the 'app' label from the pods we created before
```
kubectl label po nginx1 nginx2 nginx3 app-
# or
kubectl label po nginx{1..3} app-
# or
kubectl label po -l app app-
```
### Create a pod that will be deployed to a Node that has the label 'accelerator=nvidia-tesla-p100'
```
kubectl label nodes <your-node-name> accelerator=nvidia-tesla-p100
kubectl get nodes --show-labels
```
```
apiVersion: v1
kind: Pod
metadata:
  name: cuda-test
spec:
  containers:
    - name: cuda-test
      image: "k8s.gcr.io/cuda-vector-add:v0.1"
  nodeSelector: # add this
    accelerator: nvidia-tesla-p100 # the selection label
```
### Annotate pods nginx1, nginx2, nginx3 with "description='my description'" value
```
kubectl annotate po nginx1 nginx2 nginx3 description='my description'

#or

kubectl annotate po nginx{1..3} description='my description'
```
### Check the annotations for pod nginx1
```
kubectl annotate pod nginx1 --list
  
# or

kubectl describe po nginx1 | grep -i 'annotations'

# or

kubectl get po nginx1 -o custom-columns=Name:metadata.name,ANNOTATIONS:metadata.annotations.description
```
### Remove the annotations for these three pods
```
kubectl annotate po nginx{1..3} description-

```
### Remove these pods to have a clean state in your cluster
```
kubectl delete po nginx{1..3}

```
### Create a deployment with image nginx:1.18.0, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)
```
kubectl create deploy nginx --image=nginx:1.18.0 --replicas=2 --port=80

```
### View the YAML of this deployment
```
kubectl get deploy nginx -o yaml

```
### View the YAML of the replica set that was created by this deployment
```
kubectl get rs nginx-7bf7478b77 -o yaml

```
### Get the YAML for one of the pods
```
kubectl get po -l run=nginx # if you created deployment by 'run' command

```
### Check how the deployment rollout is going
```
kubectl rollout status deploy nginx

```
### Update the nginx image to nginx:1.19.8
```
kubectl set image deploy nginx nginx=nginx:1.19.8
# alternatively...
kubectl edit deploy nginx # change the .spec.template.spec.containers[0].image
```
### Check the rollout history and confirm that the replicas are OK
```
kubectl rollout history deploy nginx
kubectl get deploy nginx
kubectl get rs # check that a new replica set has been created
kubectl get po
```
### Undo the latest rollout and verify that new pods have the old image (nginx:1.18.0)
```
kubectl rollout undo deploy nginx
# wait a bit
kubectl get po # select one 'Running' Pod
kubectl describe po nginx-5ff4457d65-nslcl | grep -i image # should be nginx:1.18.0
```
### Do an on purpose update of the deployment with a wrong image nginx:1.91
```
kubectl set image deploy nginx nginx=nginx:1.91
# or
kubectl edit deploy nginx
# change the image to nginx:1.91
# vim tip: type (without quotes) '/image' and Enter, to navigate quickly
```
### Verify that something's wrong with the rollout
```
kubectl rollout status deploy nginx
# or
kubectl get po # you'll see 'ErrImagePull' or 'ImagePullBackOff'
```
### Return the deployment to the second revision (number 2) and verify the image is nginx:1.19.8
```
kubectl rollout undo deploy nginx --to-revision=2
kubectl describe deploy nginx | grep Image:
kubectl rollout status deploy nginx # Everything should be OK
```
### Check the details of the fourth revision (number 4)
```
kubectl rollout history deploy nginx --revision=4 # You'll also see the wrong image displayed here

```
### Scale the deployment to 5 replicas
```
kubectl scale deploy nginx --replicas=5
kubectl get po
kubectl describe deploy nginx
```
### Autoscale the deployment, pods between 5 and 10, targetting CPU utilization at 80%
```
kubectl autoscale deploy nginx --min=5 --max=10 --cpu-percent=80
# view the horizontalpodautoscalers.autoscaling for nginx
kubectl get hpa nginx
```
### Pause the rollout of the deployment
```
kubectl rollout pause deploy nginx

```
### Update the image to nginx:1.19.9 and check that there's nothing going on, since we paused the rollout
```
kubectl set image deploy nginx nginx=nginx:1.19.9
# or
kubectl edit deploy nginx
# change the image to nginx:1.19.9
kubectl rollout history deploy nginx # no new revision
```
### Resume the rollout and check that the nginx:1.19.9 image has been applied
```
kubectl rollout resume deploy nginx
kubectl rollout history deploy nginx
kubectl rollout history deploy nginx --revision=6 # insert the number of your latest revision
```
### Delete the deployment and the horizontal pod autoscaler you created
```
kubectl delete deploy nginx
kubectl delete hpa nginx

#Or
kubectl delete deploy/nginx hpa/nginx
```
### Create a job named pi with image perl that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"
```
```
### Wait till it's done, get the output
```
```
### Create a job with the image busybox that executes the command 'echo hello;sleep 30;echo world'
```
```
### Follow the logs for the pod (you'll wait for 30 seconds)
```
```
### See the status of the job, describe it and see the logs
```
```
### Delete the job
```
```
### Create a job but ensure that it will be automatically terminated by kubernetes if it takes more than 30 seconds to execute
```
```
### Create the same job, make it run 5 times, one after the other. Verify its status and delete it
```
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  completions: 5 # add this line
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: busybox
    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - echo hello;sleep 30;echo world
        image: busybox
        name: busybox
        resources: {}
      restartPolicy: OnFailure
status: {}
```
### Create the same job, but make it run 5 parallel times
```
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  parallelism: 5 # add this line
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: busybox
    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - echo hello;sleep 30;echo world
        image: busybox
        name: busybox
        resources: {}
      restartPolicy: OnFailure
status: {}
```
### Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from the Kubernetes cluster' to standard output
```
kubectl create cronjob busybox --image=busybox --schedule="*/1 * * * *" -- /bin/sh -c 'date; echo Hello from the Kubernetes cluster'

```
### See its logs and delete it
```
kubectl get cj
kubectl get jobs --watch
kubectl get po --show-labels # observe that the pods have a label that mentions their 'parent' job
kubectl logs busybox-1529745840-m867r
# Bear in mind that Kubernetes will run a new job/pod for each new cron job
kubectl delete cj busybox
```
### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it takes more than 17 seconds to start execution after its scheduled time (i.e. the job missed its scheduled time).
```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  creationTimestamp: null
  name: time-limited-job
spec:
  startingDeadlineSeconds: 17 # add this line
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: time-limited-job
    spec:
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
          - args:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
            image: busybox
            name: time-limited-job
            resources: {}
          restartPolicy: Never
  schedule: '* * * * *'
status: {}
```
### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it successfully starts but takes more than 12 seconds to complete execution.
```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  creationTimestamp: null
  name: time-limited-job
spec:
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: time-limited-job
    spec:
      activeDeadlineSeconds: 12 # add this line
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
          - args:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
            image: busybox
            name: time-limited-job
            resources: {}
          restartPolicy: Never
  schedule: '* * * * *'
status: {}
```
