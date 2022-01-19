![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/configuration&empty)
# Configuration (18%)

## ConfigMaps

### Create a configmap named config with values foo=lala,foo2=lolo

### Display its values

### Create and display a configmap from a file

### Create and display a configmap from a .env file

### Create and display a configmap from a file, giving the key 'special'


### Create a configMap called 'options' with the value var5=val5. Create a new nginx pod that loads the value from variable 'var5' in an env variable called 'option'
### Create a configMap 'anotherone' with values 'var6=val6', 'var7=val7'. Load this configMap as env variables into a new nginx pod


### Create a configMap 'cmvolume' with values 'var8=val8', 'var9=val9'. Load this as a volume inside an nginx pod on path '/etc/lala'. Create the pod and 'ls' into the '/etc/lala' directory.

## SecurityContext

### Create the YAML for an nginx pod that runs with the user ID 101. No need to create the pod


### Create the YAML for an nginx pod that has the capabilities "NET_ADMIN", "SYS_TIME" added on its single container


## Requests and limits

### Create an nginx pod with requests cpu=100m,memory=256Mi and limits cpu=200m,memory=512Mi


## Secrets

### Create a secret called mysecret with the values password=mypass


### Create a secret called mysecret2 that gets key/value from a file

### Get the value of mysecret2


### Create an nginx pod that mounts the secret mysecret2 in a volume on path /etc/foo



### Delete the pod you just created and mount the variable 'username' from secret mysecret2 onto a new nginx pod in env variable called 'USERNAME'



## ServiceAccounts


### See all the service accounts of the cluster in all namespaces


### Create a new serviceaccount called 'myuser'




### Create an nginx pod that uses 'myuser' as a service account

