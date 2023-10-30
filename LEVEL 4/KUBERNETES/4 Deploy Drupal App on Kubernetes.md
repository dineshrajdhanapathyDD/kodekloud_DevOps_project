
## Question

We need to deploy a Drupal application on Kubernetes cluster. The Nautilus application development team want to setup a fresh Drupal as they will do the installation on their own. Below you can find the requirements, they have shared with us.  
  

  

1) Configure a persistent volume  `drupal-mysql-pv`  with  `hostPath = /drupal-mysql-data`  (`/drupal-mysql-data`  directory already exists on the worker Node i.e jump host),  `5Gi`  of storage and  `ReadWriteOnce`  access mode.  
  

2) Configure one PersistentVolumeClaim named  `drupal-mysql-pvc`  with storage request of  `3Gi`  and  `ReadWriteOnce`  access mode.  
  

3) Create a deployment  `drupal-mysql`  with  `1`  replica, use  `mysql:5.7`  image. Mount the claimed PVC at  `/var/lib/mysql`.  
  

4) Create a deployment  `drupal`  with  `1`  replica and use  `drupal:8.6`  image.  
  

4) Create a  `NodePort`  type service which should be named as  `drupal-service`  and nodePort should be  `30095`.  
  

5) Create a service  `drupal-mysql-service`  to expose mysql deployment on port  `3306`.  
  

6) Set rest of the configration for deployments, services, secrets etc as per your preferences. At the end you should be able to access the Drupal installation page by clicking on  `App`  button.  
  

`Note:`  The  `kubectl`  on  `jump_host`  has been configured to work with the kubernetes cluster.


## Solution

**1. Check the path** 
```

thor@jump_host ~$ ls -la /drupal-mysql-data/
total 8
drwxr-xr-x 2 thor thor 4096 Oct 29 16:24 .
drwxr-xr-x 1 root root 4096 Oct 29 16:24 ..
 
```
**2. Create the `pv-pvc.yml` as per requirements.**
```

thor@jump_host ~$ vi pv-pvc.yml
thor@jump_host ~$ cat pv-pvc.yml
--- 
apiVersion: v1
kind: PersistentVolume
metadata:
  name: drupal-mysql-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /drupal-mysql-data
--- 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: drupal-mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi

```
**3. Apply and verify.**
```

thor@jump_host ~$ k apply -f .
persistentvolume/drupal-mysql-pv created
persistentvolumeclaim/drupal-mysql-pvc created
 
```


```
thor@jump_host ~$ k get pv,pvc
NAME                               CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
persistentvolume/drupal-mysql-pv   5Gi        RWO            Retain           Available                                   26s

NAME                                     STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/drupal-mysql-pvc   Pending                                      standard       26s

```
**4. create `drupal.yml` for the deployments and services.**
```

thor@jump_host ~$ vi drupal.yml
thor@jump_host ~$ cat drupal.yml
#### MySQL 
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal-mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: drupal-mysql
  template:
    metadata:
      labels:
        app: drupal-mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: your_password_here
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: drupal-mysql-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: drupal-mysql-storage
          persistentVolumeClaim:
            claimName: drupal-mysql-pvc
---
#### Drupal 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: drupal
spec:
  replicas: 1
  selector:
    matchLabels:
      app: drupal
  template:
    metadata:
      labels:
        app: drupal
    spec:
      containers:
        - name: drupal
          image: drupal:8.6
          ports:
            - containerPort: 80
--- 
#### NodePort Service
apiVersion: v1
kind: Service
metadata:
  name: drupal-service
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30095
  selector:
    app: drupal
--- 
#### MySQL service 
apiVersion: v1
kind: Service
metadata:
  name: drupal-mysql-service
spec:
  ports:
    - port: 3306
  selector:
    app: drupal-mysql

```
**5. Apply and verify.**

```

thor@jump_host ~$ k apply -f .
deployment.apps/drupal-mysql created
deployment.apps/drupal created
service/drupal-service created
service/drupal-mysql-service created
persistentvolume/drupal-mysql-pv unchanged
persistentvolumeclaim/drupal-mysql-pvc unchanged
```

```
thor@jump_host ~$ k get po,deployments,svc,pv,pvc
NAME                               READY   STATUS              RESTARTS   AGE
pod/drupal-5b67849c6-pd7kc         1/1     Running             0          38s
pod/drupal-mysql-c5d58677c-w44zw   0/1     ContainerCreating   0          38s

NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/drupal         1/1     1            1           38s
deployment.apps/drupal-mysql   0/1     1            0           38s

NAME                           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/drupal-mysql-service   ClusterIP   10.96.232.249   <none>        3306/TCP       38s
service/drupal-service         NodePort    10.96.76.203    <none>        80:30095/TCP   38s
service/kubernetes             ClusterIP   10.96.0.1       <none>        443/TCP        29m

NAME                                                        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                      STORAGECLASS   REASON   AGE
persistentvolume/drupal-mysql-pv                            5Gi        RWO            Retain           Available                                                      5m15s
persistentvolume/pvc-b61fbc4f-15a5-46c4-9d16-efdee458a01c   3Gi        RWO            Delete           Bound       default/drupal-mysql-pvc   standard                26s

NAME                                     STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/drupal-mysql-pvc   Bound    pvc-b61fbc4f-15a5-46c4-9d16-efdee458a01c   3Gi        RWO            standard       5m15s
 
```
**6. To verify, click the `App`  at the upper right to open the application in a new tab.**

![kube 4 4 2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/5bf5e36b-2f1b-4adb-b519-20f03abaf16a)

- **Going over to the new tab:**

![kube 4 4 1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e5360db7-1a09-423a-b52a-6d03207da014)

**7. Click on `Finish` & `Confirm` to complete the task successfully**

![kube 4 4 3](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/a6669209-77e2-4f10-8bbf-37a9a04a4303)