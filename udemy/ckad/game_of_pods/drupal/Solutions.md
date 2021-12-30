# Solution to game of pods:drupal
![drupal architecture](drupal_architecture.PNG)

##  drupal-service
'''
kubectl create service nodeport  drupal-service --tcp=80 --node-port=30095 --dry-run=client  -o yaml | kubectl set selector --local -f - 'app=drupal' -o yaml > drupal-service.yaml
kubectl apply -f drupal-service.yaml
'''

##  drupal-mysql-service

'''
kubectl create service clusterip  drupal-mysql-service --tcp=3306:80 --dry-run=client -o yaml | kubectl set selector  --local -f - 'app=drupal' -o yaml > drupal-mysql-service.yaml
kubectl apply -f drupal-mysql-service.yaml

'''

## drupal-pv

First create yaml 
'''
terminal $ cat > drupal-pv.yaml 
apiVersion: v1
kind: PersistentVolume
metadata:
  name: drupal-pv
  labels:
    app: drupaly
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: drupal-pv
  hostPath:
      path: "drupal-data"
^C
'''

Then create the pv:
'''
kubectl apply -f drupal-pv.yaml

'''

## drupal-mysql-pv

First create yaml
'''
controlplane $ cat > drupal-mysql-pv.yaml 
apiVersion: v1
kind: PersistentVolume
metadata:
  name: drupal-mysql-pv
  labels:
    app: drupaly
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: drupal-mysql-pv
  hostPath:
    path: "/drupal-mysql-data"
^C
'''

Then create pv:
'''
kubectlkl apply -f drupal-mysql-data
'''
## drupal-pvc

'''
cat > drupal-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: drupal-pvc
  labels:
    app: drupal
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 5Gi
  storageClassName: drupal-pv
^C

'''

'''

'''

