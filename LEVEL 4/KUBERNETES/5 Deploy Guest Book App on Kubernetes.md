


## Question 

The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.

  

`BACK-END TIER`

1.  Create a deployment named  `redis-master`  for Redis master.
    
    a.) Replicas count should be  `1`.
    
    b.) Container name should be  `master-redis-xfusion`  and it should use image  `redis`.
    
    c.) Request resources as  `CPU`  should be  `100m`  and  `Memory`  should be  `100Mi`.
    
    d.) Container port should be redis default port i.e  `6379`.
    
2.  Create a service named  `redis-master`  for Redis master. Port and targetPort should be Redis default port i.e  `6379`.
    
3.  Create another deployment named  `redis-slave`  for Redis slave.
    
    a.) Replicas count should be  `2`.
    
    b.) Container name should be  `slave-redis-xfusion`  and it should use  `gcr.io/google_samples/gb-redisslave:v3`  image.
    
    c.) Requests resources as  `CPU`  should be  `100m`  and  `Memory`  should be  `100Mi`.
    
    d.) Define an environment variable named  `GET_HOSTS_FROM`  and its value should be  `dns`.
    
    e.) Container port should be Redis default port i.e  `6379`.
    
4.  Create another service named  `redis-slave`. It should use Redis default port i.e  `6379`.
    

`FRONT END TIER`

1.  Create a deployment named  `frontend`.
    
    a.) Replicas count should be  `3`.
    
    b.) Container name should be  `php-redis-xfusion`  and it should use  `gcr.io/google-samples/gb-frontend:v4`  image.
    
    c.) Request resources as  `CPU`  should be  `100m`  and  `Memory`  should be  `100Mi`.
    
    d.) Define an environment variable named as  `GET_HOSTS_FROM`  and its value should be  `dns`.
    
    e.) Container port should be  `80`.
    
2.  Create a service named  `frontend`. Its  `type`  should be  `NodePort`, port should be  `80`  and its  `nodePort`  should be  `30009`.
    

Finally, you can check the  `guestbook app`  by clicking on  `App`  button.  
  

`You can use any labels as per your choice.`

`Note:`  The  `kubectl`  utility on  `jump_host`  has been configured to work with the kubernetes cluster.


## Solution

**1. At first kubectl utility configure and working from jump server, run below commands**

```

thor@jump_host ~$ kubectl get deploy
No resources found in default namespace.
thor@jump_host ~$ kubectl get services
No resources found in default namespace.
thor@jump_host ~$ kubectl get pods
No resources found in default namespace.

```
**2 . Multiple deployments and service YAML files rather than one single file. Created 6 files and shown how to pull from Git repo.**

- Check `Git` version
```
thor@jump_host ~$ git version
bash: git: command not found
```
- Install `Git` 
```
thor@jump_host ~$ sudo yum install git -y

Complete!
thor@jump_host ~$ git version
git version 2.39.3

thor@jump_host ~$ cd /tmp/
thor@jump_host /tmp$ ls
demofile2.json  ks-script-blvdp0_4  ks-script-ei2y7edh

thor@jump_host /tmp$ ls -lsd
4 drwxrwxrwt 1 root root 4096 Oct 30 17:53 .
thor@jump_host /tmp$ 

```
- `Git` Clone

```

thor@jump_host /tmp$ git clone https://gitlab.com/nb-tech-support/devops.git
Cloning into 'devops'...
remote: Enumerating objects: 444, done.
remote: Counting objects: 100% (332/332), done.
remote: Compressing objects: 100% (173/173), done.
remote: Total 444 (delta 157), reused 332 (delta 157), pack-reused 112
Receiving objects: 100% (444/444), 63.24 KiB | 12.65 MiB/s, done.
Resolving deltas: 100% (207/207), done.

```
- Check directory 


```

thor@jump_host /tmp$ ls
demofile2.json  devops  ks-script-blvdp0_4  ks-script ei2y7edh

thor@jump_host /tmp$ cd devops
thor@jump_host /tmp/devops$ ls
' updated with service exposeNginx and Phpfpm on Kubernetes'
 Ansible
 CHANGELOG
 CHANGELOG.md
'Countdown job in Kubernetes'
'Create Cronjobs in Kubernetes'
'Deploy Apache Web Server on Kubernetes CLuster'
'Deploy Grafana on Kubernetes Cluster'
'Deploy Haproxy App on Kubernetes'
'Deploy Jekyll App on Kubernetes'
'Deploy Jenkins on Kubernetes'
'Deploy Lamp Stack on Kubernetes Cluster'
'Deploy MySQL on Kubernetes'
'Deploy Nginx Web Server on Kubernetes Cluster'
'Deploy Nginx and Phpfpm on Kubernetes'
'Deploy Node App on Kubernetes'
'Deploy Redis Cluster on Kubernetes'
'Deploy Tomcat App on Kubernetes'
 Deploy_Guest_Book_App_on_Kubernetes
 Deploy_Iron_Gallery_App_on_Kubernetes
 Deploy_Jenkins_on_Kubernetes.yaml
'Docker stats,  cAdvisor, Grafana with Prometheus for Alerting and Monitoring'
'Environment Variables in Kubernetes'
'Grafana with Prometheus for Alerting and Monitoring'
'Init Containers in Kubernetes'
'Kubernetes Redis Deployment'
'Kubernetes Shared Volumes'
'Kubernetes Sidecar Containers'
'Kubernetes Time Check Pod'
'Kubernetes Time Check Pod with ENV'
'Kubernetes Troubleshooting '
'Linux challenge series'
'Nginx with mysql'
'Node Affinity in Kubernetes Cluster'
'Persistent Volumes in Kubernetes'
'Persistent Volumes in Kubernetes HTTPD Pod'
 Puppet
 README.md
'Rolling Updates And Rolling Back Deployments in Kubernete'
'Rolling Updates And Rolling Back Deployments with NodePort'
'Set Limits for Resources in Kubernetes'
 kubernetes-challenges

```
- Find the 6 files directory
```
thor@jump_host /tmp/devops$ cd Deploy_Guest_Book_App_on_Kubernetes

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ ls
Back-end-Deploy-Redis-Master-Guest-Book-App.yaml   
Back-end-service-Redis-slave-Guest-Book-App.yaml
Back-end-Deploy-Redis-slave-Guest-Book-App.yaml    
Front-end-Deploy-Redis-php-Guest-Book-App.yaml
Back-end-service-Redis-Master-Guest-Book-App.yaml  
Front-end-service-Redis-php-Guest-Book-App.yaml

```
![70](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/78a30ecc-9f57-4ba5-af8d-34a9672e305b)

## `BACK-END TIER`

**3. Create a deployment named  `redis-master`  for Redis master.**
```

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ vi Back-end-Deploy-Redis-Master-Guest-Book-App.yaml

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ cat Back-end-Deploy-Redis-Master-Guest-Book-App.yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis-master
      tier: back-end
  template:
    metadata:
      labels:
        app: redis-master
        tier: back-end
    spec:
      containers:
        - name: master-redis-xfusion
          image: redis
          resources:
            requests:
              memory: "100Mi"
              cpu: "100m"
          ports:
            - containerPort: 6379
            
```
```

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ kubectl apply -f Back-end-Deploy-Redis-Master-Guest-Book-App.yaml
deployment.apps/redis-master created

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
redis-master-8485f9466c-92ph4   1/1     Running   0          35s

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ kubectl get service
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   43m

```

**4. Create a service named  `redis-master`  for Redis master.**

```

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ vi Back-end-service-Redis-Master-Guest-Book-App.yaml

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ cat Back-end-service-Redis-Master-Guest-Book-App.yaml
---
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  type: ClusterIP
  selector:
    app: redis-master
    tier: back-end
  ports:
    - port: 6379
      targetPort: 6379

```
```

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ kubectl apply -f Back-end-service-Redis-Master-Guest-Book-App.yaml
service/redis-master created 

thor@jump_host /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes$ cd /

thor@jump_host /$ kubectl get service
NAME           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
kubernetes     ClusterIP   10.96.0.1      <none>        443/TCP    47m
redis-master   ClusterIP   10.96.212.54   <none>        6379/TCP   63s 

```

**5. Create another deployment named  `redis-slave`  for Redis slave.**

```

thor@jump_host /$ vi /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Back-end-Deploy-Redis-slave-Guest-Book-App.yaml
thor@jump_host /$ cat /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Back-end-Deploy-Redis-slave-Guest-Book-App.yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-slave
      tier: back-end
  template:
    metadata:
      labels:
        app: redis-slave
        tier: back-end
    spec:
      containers:
        - name: slave-redis-xfusion
          image: gcr.io/google_samples/gb-redisslave:v3
          resources:
            requests:
              memory: "100Mi"
              cpu: "100m"
          env:
            - name: GET_HOSTS_FROM
              value: dns
          ports:
            - containerPort: 6379
            
```
``` 

thor@jump_host /$ kubectl get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
redis-master   1/1     1            1           12m

thor@jump_host /$ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
redis-master-8485f9466c-92ph4   1/1     Running   0          13m

thor@jump_host /$ kubectl apply -f /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Back-end-Deploy-Redis-slave-Guest-Book-App.yaml
deployment.apps/redis-slave created

thor@jump_host /$ kubectl get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
redis-master   1/1     1            1           14m
redis-slave    0/2     2            0           5s

thor@jump_host /$ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
redis-master-8485f9466c-92ph4   1/1     Running   0          14m
redis-slave-55748748cd-46rtf    1/1     Running   0          13s
redis-slave-55748748cd-jm87n    1/1     Running   0          13s
 
```

**6. Create another service named  `redis-slave`.**

```

thor@jump_host /$ vi /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Back-end-service-Redis-slave-Guest-Book-App.yaml

thor@jump_host /$ cat /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Back-end-service-Redis-slave-Guest-Book-App.yaml
---
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  type: ClusterIP
  selector:
    app: redis-slave
    tier: back-end
  ports:
    - port: 6379
      targetPort: 6379

```
```

thor@jump_host /$ kubectl apply -f /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Back-end-service-Redis-slave-Guest-Book-App.yaml
service/redis-slave created

thor@jump_host /$ kubectl get service
NAME           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
kubernetes     ClusterIP   10.96.0.1      <none>        443/TCP    60m
redis-master   ClusterIP   10.96.212.54   <none>        6379/TCP   13m
redis-slave    ClusterIP   10.96.78.97    <none>        6379/TCP   12s
 
```

## `FRONT END TIER`

**7. Create a deployment named  `frontend`.**

```

thor@jump_host /$ vi /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Front-end-Deploy-Redis-php-Guest-Book-App.yaml

thor@jump_host /$ cat /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Front-end-Deploy-Redis-php-Guest-Book-App.yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
      tier: front-end
  template:
    metadata:
      labels:
        app: guestbook
        tier: front-end
    spec:
      containers:
        - name: php-redis-xfusion
          image: gcr.io/google-samples/gb-frontend:v4
          resources:
            requests:
              memory: "100Mi"
              cpu: "100m"
          env:
            - name: GET_HOSTS_FROM
              value: dns
          ports:
            - containerPort: 80

```
```

thor@jump_host /$ kubectl apply -f tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Front-end-Deploy-Redis-php-Guest-Book-App.yaml
deployment.apps/frontend created

thor@jump_host /$ kubectl get pods
NAME                            READY   STATUS              RESTARTS   AGE
frontend-8fb89bcfc-6tlrb        0/1     ContainerCreating   0          11s
frontend-8fb89bcfc-7lp5w        0/1     ContainerCreating   0          11s
frontend-8fb89bcfc-qttgc        0/1     ContainerCreating   0          11s
redis-master-8485f9466c-92ph4   1/1     Running             0          22m
redis-slave-55748748cd-46rtf    1/1     Running             0          8m57s
redis-slave-55748748cd-jm87n    1/1     Running             0          8m57s

thor@jump_host /$ kubectl get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
frontend       0/3     3            0           25s
redis-master   1/1     1            1           23m
redis-slave    2/2     2            2           9m11s

thor@jump_host /$ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
frontend-8fb89bcfc-6tlrb        1/1     Running   0          50s
frontend-8fb89bcfc-7lp5w        1/1     Running   0          50s
frontend-8fb89bcfc-qttgc        1/1     Running   0          50s
redis-master-8485f9466c-92ph4   1/1     Running   0          23m
redis-slave-55748748cd-46rtf    1/1     Running   0          9m36s
redis-slave-55748748cd-jm87n    1/1     Running   0          9m36s

thor@jump_host /$ kubectl get deploy
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
frontend       3/3     3            3           54s
redis-master   1/1     1            1           23m
redis-slave    2/2     2            2           9m40s
 
```
**8. Create a service named  `frontend`.**

```

thor@jump_host /$ vi /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Front-end-service-Redis-php-Guest-Book-App.yaml

thor@jump_host /$ cat /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Front-end-service-Redis-php-Guest-Book-App.yaml
---
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
    app: guestbook
    tier: front-end
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30009

```
```

thor@jump_host /$ kubectl get service
NAME           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
kubernetes     ClusterIP   10.96.0.1      <none>        443/TCP    68m
redis-master   ClusterIP   10.96.212.54   <none>        6379/TCP   21m
redis-slave    ClusterIP   10.96.78.97    <none>        6379/TCP   7m56s

thor@jump_host /$ kubectl apply -f /tmp/devops/Deploy_Guest_Book_App_on_Kubernetes/Front-end-service-Redis-php-Guest-Book-App.yaml
service/frontend created

thor@jump_host /$ kubectl get service
NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
frontend       NodePort    10.96.218.145   <none>        80:30009/TCP   16s
kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP        68m
redis-master   ClusterIP   10.96.212.54    <none>        6379/TCP       22m
redis-slave    ClusterIP   10.96.78.97     <none>        6379/TCP       8m32s

``` 

**9. Running status of pods**
```
thor@jump_host /$ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
frontend-8fb89bcfc-6tlrb        1/1     Running   0          3m56s
frontend-8fb89bcfc-7lp5w        1/1     Running   0          3m56s
frontend-8fb89bcfc-qttgc        1/1     Running   0          3m56s
redis-master-8485f9466c-92ph4   1/1     Running   0          26m
redis-slave-55748748cd-46rtf    1/1     Running   0          12m
redis-slave-55748748cd-jm87n    1/1     Running   0          12m
```
**10. Validate the task by curl and open the browser by clicking `APPS`**
```

thor@jump_host /$ kubectl exec frontend-8fb89bcfc-6tlrb -- curl -I http://localhost/ 
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0HTTP/1.1 200 OK
Date: Mon, 30 Oct 2023 18:40:55 GMT
Server: Apache/2.4.10 (Debian) PHP/5.6.20
Last-Modified: Wed, 09 Sep 2015 18:35:04 GMT
ETag: "399-51f54bdb4a600"
Accept-Ranges: bytes
Content-Length: 921
Vary: Accept-Encoding
Content-Type: text/html

  0   921    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
```
![68](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/804e8f36-0428-4518-be61-cd8a4a2348bd)



 **11. Check the `guestbook app` by clicking on `App` button.**

![67](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c63b542a-d094-44eb-9d7f-9d9ba14f57e8)


**12. Click on `Finish` & `Confirm` to complete the task successful**

![69](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/40944bfe-b3f1-4a60-b827-07f74615227d)