

## Question

The Nautilus application development team observed some performance issues with one of the application that is deployed in Kubernetes cluster. After looking into number of factors, the team has suggested to use some in-memory caching utility for DB service. After number of discussions, they have decided to use Redis. Initially they would like to deploy Redis on kubernetes cluster for testing and later they will move it to production. Please find below more details about the task:

  

Create a redis deployment with following parameters:

1.  Create a  `config map`  called  `my-redis-config`  having  `maxmemory 2mb`  in  `redis-config`.
    
2.  Name of the  `deployment`  should be  `redis-deployment`, it should use  
    `redis:alpine`  image and container name should be  `redis-container`. Also make sure it has only  `1`  replica.
    
3.  The container should request for  `1`  CPU.
    
4.  Mount  `2`  volumes:
    

-  a. An Empty directory volume called  `data`  at path  `/redis-master-data`.

- b. A configmap volume called  `redis-config`  at path  `/redis-master`.

 - c. The container should expose the port  `6379`.

5.  Finally,  `redis-deployment`  should be in an up and running state.

`Note:`  The  `kubectl`  utility on  `jump_host`  has been configured to work with the kubernetes cluster.


## Solution

**1. Check the resources.**

```

thor@jump_host ~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   23m

```

**2. Create a YAML file **deploy.yml** based on the requirements.** 
```

thor@jump_host ~$ vi deploy.yml
thor@jump_host ~$ cat deploy.yml
---
kind: ConfigMap
apiVersion: v1
metadata:
  name: my-redis-config
data:
  maxmemory: 2mb
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis-container
          image: redis:alpine
          ports:
            - containerPort: 6379
          resources:
            requests:
              cpu: "1000m"
          volumeMounts:
            - name: data
              mountPath: /redis-master-data
            - name: redis-config
              mountPath: /redis-master
      volumes:
      - name: data
        emptyDir: {}
      - name: redis-config
        configMap:
          name: my-redis-config 

```
**3. Apply.**
```

thor@jump_host ~$ kubectl apply -f .
configmap/my-redis-config created
deployment.apps/redis-deployment created
 
```
**4. Check the resources again.**

```
thor@jump_host ~$  kubectl get pod,deployment,cm
NAME                                   READY   STATUS    RESTARTS   AGE
pod/redis-deployment-68fbd4467-gtw5r   1/1     Running   0          39s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/redis-deployment   1/1     1            1           39s

NAME                         DATA   AGE
configmap/kube-root-ca.crt   1      26m
configmap/my-redis-config    1      39s
thor@jump_host ~$ 
```
**5. Click on Finish & Confirm to complete the task successfully**

![kub](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9b3c0de5-68e0-4293-a073-5a3548671a71)