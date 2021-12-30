# Solution to game of pods:drupal
![drupal architecture](drupal_architecture.PNG)

## drupal-secret
```
kubectl create secret generic drupal-mysql-secret --from-literal=MYSQL_ROOT_PASSWORD=root_password --from-literal=MYSQL_DATABASE=drupal-database --from-literal=MYSQL_USER=root
```

##  drupal-service
```
kubectl create service nodeport  drupal-service --tcp=80 --node-port=30095 --dry-run=client  -o yaml | kubectl set selector --local -f - 'app=drupal' -o yaml > drupal-service.yaml
```
```
kubectl apply -f drupal-service.yaml
```



##  drupal-mysql-service

```
kubectl create service clusterip  drupal-mysql-service --tcp=3306:80 --dry-run=client -o yaml | kubectl set selector  --local -f - 'app=drupal' -o yaml > drupal-mysql-service.yaml
```
```
kubectl apply -f drupal-mysql-service.yaml
```

## drupal-pv

First create yaml 
```
touch  drupal-pv.yaml 
```

Then edit the yaml

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: drupal-pv
  labels:
    app: drupal
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: drupal-pv
  hostPath:
      path: "/drupal-data"
```


Then create the pv:
```
kubectl apply -f drupal-pv.yaml
```

## drupal-mysql-pv

First create yaml
```
touch  drupal-mysql-pv.yaml 
```

Then edit the yaml
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: drupal-mysql-pv
  labels:
    app: drupal
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: drupal-mysql-data
  hostPath:
    path: "/drupal-mysql-data"
```

Then create pv:

```
kubectl apply -f drupal-mysql-pv.yaml
```

## drupal-pvc

```
touch  drupal-pvc.yaml
```


```
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
```


Then 


```
kubectl apply -f drupal-pvc.yaml

```


## drupal-mysql-pvc

```
touch  drupal-mysql-pvc.yaml

```
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: drupal-mysql-pvc
  labels:
    app: drupal
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 5Gi
  storageClassName: drupal-mysql-data
```


Then 


```
kubectl apply -f drupal-mysql-pvc.yaml

```

## drupal-deployment

1.
```
touch drupal-deployment.yaml
```

2.
edit the file:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: drupal
  name: drupal
spec:
  replicas: 1
  selector:
    matchLabels:
      app: drupal
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: drupal
    spec:
      containers:
      - image: drupal:8.6
        name: drupal
        ports:
        - containerPort: 80
        volumeMounts:
          - mountPath: /var/www/html/modules
            name: drupal-data
            subPath: modules
          - mountPath: /var/www/html/profiles
            name: drupal-data
            subPath: profiles
          - mountPath: /var/www/html/sites
            name: drupal-data
            subPath: sites
          - mountPath: /var/www/html/themes
            name: drupal-data
            subPath: themes
      initContainers:
      - name: init-sites-volume
        image: drupal:8.6
        command: ["/bin/bash","-c"]
        args: ["cp -r /var/www/html/sites/ /data/; chown www-data:www-data /data/ -R"]
        volumeMounts:
        - name: drupal-data
          mountPath: /data
      volumes:
      - name: drupal-data
        persistentVolumeClaim:
          claimName: drupal-pvc
          
```

Then :
```
kubectl apply -f drupal-deployment.yaml
```


## drupal-mysql-deployment

```
touch drupal-mysql-deployment.yaml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: drupal-mysql
  name: drupal-mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: drupal-mysql
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: drupal-mysql
    spec:
      containers:
      - image: mysql:5.7
        name: mysql
        volumeMounts:
        - mountPath:  /var/lib/mysql
          subPath: dbdata
          name: drupal-mysql-data
      volumes:
      - name: drupal-mysql-data
        persistentVolumeClaim:
          claimName: drupal-mysql-pvc
```


```
kubectl apply -f  drupal-mysql-deployment.yaml
```
