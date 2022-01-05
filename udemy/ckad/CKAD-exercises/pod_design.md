### Create 3 pods with names nginx1,nginx2,nginx3. All of them should have the label app=v1
```
```
### Show all labels of the pods
```
```
### Change the labels of pod 'nginx2' to be app=v2
```
```
### Get the label 'app' for the pods (show a column with APP labels)
```
```
### Get only the 'app=v2' pods
```
```
### Remove the 'app' label from the pods we created before
```
```
### Create a pod that will be deployed to a Node that has the label 'accelerator=nvidia-tesla-p100'
```
```
### Annotate pods nginx1, nginx2, nginx3 with "description='my description'" value
```
```
### Check the annotations for pod nginx1
```
```
### Remove the annotations for these three pods
```
```
### Remove these pods to have a clean state in your cluster
```
```
### Create a deployment with image nginx:1.18.0, called nginx, having 2 replicas, defining port 80 as the port that this container exposes (don't create a service for this deployment)
```
```
### View the YAML of this deployment
```
```
### View the YAML of the replica set that was created by this deployment
```
```
### Get the YAML for one of the pods
```
```
### Check how the deployment rollout is going
```
```
### Update the nginx image to nginx:1.19.8
```
```
### Check the rollout history and confirm that the replicas are OK
```
```
### Undo the latest rollout and verify that new pods have the old image (nginx:1.18.0)
```
```
### Do an on purpose update of the deployment with a wrong image nginx:1.91
```
```
### Verify that something's wrong with the rollout
```
```
### Return the deployment to the second revision (number 2) and verify the image is nginx:1.19.8
```
```
### Check the details of the fourth revision (number 4)
```
```
### Scale the deployment to 5 replicas
```
```
### Autoscale the deployment, pods between 5 and 10, targetting CPU utilization at 80%
```
```
### Pause the rollout of the deployment
```
```
### Update the image to nginx:1.19.9 and check that there's nothing going on, since we paused the rollout
```
```
### Resume the rollout and check that the nginx:1.19.9 image has been applied
```
```
### Delete the deployment and the horizontal pod autoscaler you created
```
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
```
### Create the same job, but make it run 5 parallel times
```
```
### Create a cron job with image busybox that runs on a schedule of "*/1 * * * *" and writes 'date; echo Hello from the Kubernetes cluster' to standard output
```
```
### See its logs and delete it
```
```
### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it takes more than 17 seconds to start execution after its scheduled time (i.e. the job missed its scheduled time).
```
```
### Create a cron job with image busybox that runs every minute and writes 'date; echo Hello from the Kubernetes cluster' to standard output. The cron job should be terminated if it successfully starts but takes more than 12 seconds to complete execution.
```
```
