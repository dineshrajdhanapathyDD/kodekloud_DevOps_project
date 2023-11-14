

## Questions:

One of the Nautilus DevOps team members was working on to update an existing Kubernetes template. Somehow, he made some mistakes in the template and it is failing while applying. We need to fix this as soon as possible, so take a look into it and make sure you are able to apply it without any issues. Also, do not remove any component from the template like `pods/deployments/volumes` etc.


`/home/thor/mysql_deployment.yml` is the template that needs to be fixed.


`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:

**1. Check existing running Pods  & Services** 

```

thor@jump_host ~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   31m
thor@jump_host ~$ 
```

**2. Secrets is by default created in lab**

```

thor@jump_host ~$ kubectl get secrets
NAME              TYPE     DATA   AGE
mysql-db-url      Opaque   1      3m21s
mysql-root-pass   Opaque   1      3m21s
mysql-user-pass   Opaque   2      3m21s
thor@jump_host ~$ 
```

**3. To identify the issue in template , split in to each kind and troubleshoot further** 

```

thor@jump_host ~$  cp mysql_deployment.yml pv.yml
thor@jump_host ~$  cp mysql_deployment.yml pvc.yml
thor@jump_host ~$ cp mysql_deployment.yml service.yml
thor@jump_host ~$ cp mysql_deployment.yml deploy.yml

thor@jump_host ~$ ls
deploy.yml  mysql_deployment.yml  pv.yml  pvc.yml  service.yml
thor@jump_host ~$ 
```

**4. Create a YAML  file with all the parameters,**

- a) Lets fixed for pv, delete the lines of rest other kind - PersistentVolumeClaim in vi editor 

```

thor@jump_host ~$ vi pv.yml
thor@jump_host ~$ cat pv.yml
---
apiVersion: v1 
kind: PersistentVolumeClaim 
metadata:
  name: mysql-pv
  labels:
    type: local
spec:
  storageClassName: standard       
  capacity:
    storage: 250Mi
  accessModes: 
     -  ReadWriteOnce 
  hostPath:                       
    path: "/mnt/data"
  persistentVolumeReclaimPolicy: Retain  


thor@jump_host ~$ kubectl create -f pv.yml
persistentvolume/mysql-pv created

thor@jump_host ~$ kubectl get pv
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
mysql-pv   250Mi      RWO            Retain           Available           standard                49s
```

- b) Lets fixed for pvc, delete the lines of rest other kind- PersistentVolumeClaim  in vi editor  

```

thor@jump_host ~$ vi pvc.yml
thor@jump_host ~$ cat pvc.yml
---
apiVersion: v1 
kind: PersistentVolumeClaim       
metadata:                          
  name: mysql-pv-claim
  labels:
    type: mysql-app 
spec:                               
  storageClassName: standard       
  accessModes: 
  - ReadWriteOnce             
  resources:
    requests:
      storage: 250Mi 


thor@jump_host ~$ kubectl create -f pvc.yml
persistentvolumeclaim/mysql-pv-claim created

thor@jump_host ~$ kubectl get pvc
NAME             STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pv-claim   Pending                                      standard       68s
thor@jump_host ~$ 
```

- c) Lets fixed for service, delete the lines of rest other kind- Service   in vi editor  

```

thor@jump_host ~$ vi service.yml
thor@jump_host ~$ cat service.yml
---
apiVersion: v1                    
kind: Service                      
metadata:
  name: mysql         
  labels:             
    app: mysql-app  
spec:
  type: NodePort
  ports:
    - targetPort: 3306
      port: 3306
      nodePort: 30011
  selector:                       
    app: mysql_app
    tier: mysql

thor@jump_host ~$ kubectl create -f service.yml
service/mysql created
thor@jump_host ~$ 

thor@jump_host ~$ kubectl get svc
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          53m
mysql        NodePort    10.96.167.35   <none>        3306:30011/TCP   40s
thor@jump_host ~$ 
```

- d) Lets fixed for service, delete the lines of rest other kind- Deployment in vi editor  

```

thor@jump_host ~$ vi deploy.yml
thor@jump_host ~$ cat deploy.yml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql-app
spec:
  selector:
    matchLabels:
      app: mysql-app
      tier: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql-app
        tier: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
           claimName: mysql-pv-claim

thor@jump_host ~$ kubectl create -f deploy.yml
deployment.apps/mysql-deployment created
thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
mysql-deployment   1/1     1            1           42s
thor@jump_host ~$ 
```

- e) Wait for deployment & pod ready and running status

```

thor@jump_host ~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-944888cd8-5886q   1/1     Running   0          71s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          57m
service/mysql        NodePort    10.96.167.35   <none>        3306:30011/TCP   4m5s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           71s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-944888cd8   1         1         1       71s


thor@jump_host ~$ kubectl get pvc
NAME             STATUS   VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pv-claim   Bound    mysql-pv   250Mi      RWO            standard       8m32s
thor@jump_host ~$ 


thor@jump_host ~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-944888cd8-5886q   1/1     Running   0          2m57s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          58m
service/mysql        NodePort    10.96.167.35   <none>        3306:30011/TCP   5m51s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           2m57s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-944888cd8   1         1         1       2m57s
thor@jump_host ~$ 


thor@jump_host ~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-944888cd8-5886q   1/1     Running   0          4m20s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          60m
service/mysql        NodePort    10.96.167.35   <none>        3306:30011/TCP   7m14s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           4m20s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-944888cd8   1         1         1       4m20s

we have fixed all the kind issue by spliting the existing deployment templates
```

- f) To proceed further, we need to used same template single deployment file. so lets delete the existing deployment, service, pvc, & pv

```

thor@jump_host ~$ kubectl delete deploy mysql-deployment
deployment.apps "mysql-deployment" deleted
thor@jump_host ~$ kubectl delete service mysql
service "mysql" deleted

thor@jump_host ~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   62m

thor@jump_host ~$ kubectl get pvc
NAME             STATUS   VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pv-claim   Bound    mysql-pv   250Mi      RWO            standard       13m

thor@jump_host ~$ kubectl delete pvc mysql-pv-claim
persistentvolumeclaim "mysql-pv-claim" deleted

thor@jump_host ~$ kubectl get pv
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS     CLAIM                    STORAGECLASS   REASON   AGE
mysql-pv   250Mi      RWO            Retain           Released   default/mysql-pv-claim   standard                17m

thor@jump_host ~$ kubectl delete pv mysql-pv
persistentvolume "mysql-pv" deleted

thor@jump_host ~$ kubectl get pv
No resources found

thor@jump_host ~$ kubectl get pvc
No resources found in default namespace.

thor@jump_host ~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   66m

we have deleted all the splited deployment
```

- g) Clear the data from template mysql_deployment yaml file in vi editor

```

thor@jump_host ~$ vi mysql_deployment.yml
thor@jump_host ~$ cat mysql_deployment.yml


Now lets merge all splited kind files in to mysql_deployment template 

thor@jump_host ~$ cat pv.yml pvc.yml service.yml deploy.yml >> mysql_deployment.yml
thor@jump_host ~$ 

thor@jump_host ~$ cat mysql_deployment.yml
---
apiVersion: v1 
kind: PersistentVolume 
metadata:
  name: mysql-pv
  labels:
    type: local
spec:
  storageClassName: standard       
  capacity:
    storage: 250Mi
  accessModes: 
     -  ReadWriteOnce 
  hostPath:                       
    path: "/mnt/data"
  persistentVolumeReclaimPolicy: Retain  


---
apiVersion: v1 
kind: PersistentVolumeClaim       
metadata:                          
  name: mysql-pv-claim
  labels:
    type: mysql-app 
spec:                               
  storageClassName: standard       
  accessModes: 
  - ReadWriteOnce             
  resources:
    requests:
      storage: 250Mi 


---
apiVersion: v1                    
kind: Service                      
metadata:
  name: mysql         
  labels:             
    app: mysql-app  
spec:
  type: NodePort
  ports:
    - targetPort: 3306
      port: 3306
      nodePort: 30011
  selector:                       
    app: mysql_app
    tier: mysql

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql-app
spec:
  selector:
    matchLabels:
      app: mysql-app
      tier: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql-app
        tier: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-root-pass
              key: password
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: mysql-db-url
              key: database
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: username
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-user-pass
              key: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
           claimName: mysql-pv-claim

thor@jump_host ~$ 

thor@jump_host ~$ kubectl create -f mysql_deployment.yml
persistentvolume/mysql-pv created
persistentvolumeclaim/mysql-pv-claim created
service/mysql created
deployment.apps/mysql-deployment created
```
 
- h) All kinds should get deployed in one go

```

thor@jump_host ~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-944888cd8-dnvdw   1/1     Running   0          14s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          72m
service/mysql        NodePort    10.96.220.29   <none>        3306:30011/TCP   14s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           14s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-944888cd8   1         1         1       14s

thor@jump_host ~$ kubectl get pv
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                    STORAGECLASS   REASON   AGE
mysql-pv   250Mi      RWO            Retain           Bound    default/mysql-pv-claim   standard                47s

thor@jump_host ~$ kubectl get pvc
NAME             STATUS   VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pv-claim   Bound    mysql-pv   250Mi      RWO            standard       54s


thor@jump_host ~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-944888cd8-dnvdw   1/1     Running   0          76s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          73m
service/mysql        NodePort    10.96.220.29   <none>        3306:30011/TCP   76s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           76s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-944888cd8   1         1         1       76s
thor@jump_host ~$ 
```

**5. Click on `Finish` & `Confirm` to complete the task successfully**
