


## Question

A new MySQL server needs to be deployed on Kubernetes cluster. The Nautilus DevOps team was working on to gather the requirements. Recently they were able to finalize the requirements and shared them with the team members to start working on it. Below you can find the details:  
  

  

1.) Create a PersistentVolume  `mysql-pv`, its capacity should be  `250Mi`, set other parameters as per your preference.  
  

2.) Create a PersistentVolumeClaim to request this PersistentVolume storage. Name it as  `mysql-pv-claim`  and request a  `250Mi`  of storage. Set other parameters as per your preference.  
  

3.) Create a deployment named  `mysql-deployment`, use any mysql image as per your preference. Mount the PersistentVolume at mount path  `/var/lib/mysql`.  
  

4.) Create a  `NodePort`  type service named  `mysql`  and set nodePort to  `30007`.  
  

5.) Create a secret named  `mysql-root-pass`  having a key pair value, where key is  `password`  and its value is  `YUIidhb667`, create another secret named  `mysql-user-pass`  having some key pair values, where frist key is  `username`  and its value is  `kodekloud_rin`, second key is  `password`  and value is  `Rc5C9EyvbU`, create one more secret named  `mysql-db-url`, key name is  `database`  and value is  `kodekloud_db7`  
  

6.) Define some Environment variables within the container:  
  

a)  `name: MYSQL_ROOT_PASSWORD`, should pick value from secretKeyRef  `name: mysql-root-pass`  and  `key: password`  
  

b)  `name: MYSQL_DATABASE`, should pick value from secretKeyRef  `name: mysql-db-url`  and  `key: database`  
  

c)  `name: MYSQL_USER`, should pick value from secretKeyRef  `name: mysql-user-pass`  key  `key: username`  
  

d)  `name: MYSQL_PASSWORD`, should pick value from secretKeyRef  `name: mysql-user-pass`  and  `key: password`  
  

`Note:`  The  `kubectl`  utility on  `jump_host`  has been configured to work with the kubernetes cluster.

## Solution

**1. Check existing running Pods & Services**

```

thor@jump_host ~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   25m
thor@jump_host ~$ 

```
**2. Create a Secret as per the task change the username & password**
```

thor@jump_host ~$ kubectl create secret generic mysql-root-pass --from-literal=password=YUIidhb667
secret/mysql-root-pass created

thor@jump_host ~$ kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_rin --from-literal=password=Rc5C9EyvbU
secret/mysql-user-pass created
thor@jump_host ~$ kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db7
secret/mysql-db-url created

```


**3. Validate the created secret**
```

thor@jump_host ~$ kubectl get secret
NAME              TYPE     DATA   AGE
mysql-db-url      Opaque   1      38s
mysql-root-pass   Opaque   1      6m
mysql-user-pass   Opaque   2      88s
thor@jump_host ~$ 

```

**4. Create a YAML file with all the parameters like Deployment Name, Volume Name, Image Name, Replicas, Service Name, Mount Path & Ports.**
```

thor@jump_host ~$ vi /tmp/mysql.yaml
thor@jump_host ~$ cat /tmp/mysql.yaml
---                                                                                           
apiVersion: v1                                                                                
kind: PersistentVolume                                                                        
metadata:                                                                                     
  name: mysql-pv                                                                              
spec:                                                                                         
  capacity:                                                                                   
    storage: 250Mi     
  accessModes:                                                                                
    - ReadWriteOnce                                                                           
  hostPath:                                                                                   
    path: "/var/lib/mysql"                                                                    
---                                                                                           
apiVersion: v1                                                                                
kind: PersistentVolumeClaim                                                                   
metadata:                                                                                     
  name: mysql-pv-claim                                                                        
spec:                                                                                         
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
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
    - port: 3306
      targetPort: 3306
      nodePort: 30007
---       
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      volumes:
      - name: mysql-pv
        persistentVolumeClaim:
          claimName: mysql-pv-claim
      containers:
      - name: mysql
        image: mysql:latest
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
        volumeMounts:
        - name: mysql-pv
          mountPath: /var/lib/mysql
        ports:
        - containerPort: 3306
          name: mysql

```

**5. Created for mysql-pv, mysql-pv-claim, mysql & mysql-deployment.**
```

thor@jump_host ~$ kubectl create -f /tmp/mysql.yaml
persistentvolume/mysql-pv created
persistentvolumeclaim/mysql-pv-claim created
service/mysql created
deployment.apps/mysql-deployment created
 
```
**6. Wait for deployment & pods running**

```

thor@jump_host ~$ kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                    STORAGECLASS   REASON   AGE
mysql-pv                                   250Mi      RWO            Retain           Available                                                    46s
pvc-a48180df-628c-4b85-a4d7-cf12af117e20   250Mi      RWO            Delete           Bound       default/mysql-pv-claim   standard                41s
thor@jump_host ~$ kubectl get pvc
NAME             STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mysql-pv-claim   Bound    pvc-a48180df-628c-4b85-a4d7-cf12af117e20   250Mi      RWO            standard       56s
thor@jump_host ~$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/mysql-deployment-8b94c65c8-z89vp   1/1     Running   0          66s

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP          42m
service/mysql        NodePort    10.96.151.222   <none>        3306:30007/TCP   66s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mysql-deployment   1/1     1            1           66s

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/mysql-deployment-8b94c65c8   1         1         1       66s
thor@jump_host ~$ 
```
**7. Environment variables within the container**

```
thor@jump_host ~$ kubectl exec -it mysql-deployment-8b94c65c8-z89vp -- bin/bash
bash-4.4# 
```
![kub 4 2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4381f1fb-be83-47e2-855e-f3fb237d00cf)

 **8. Click on Finish & Confirm to complete the task successfully**

![kub 4 2 1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/aba8e2d1-f5d9-43e7-a6ad-c84525a38d8b)
