


## Questions:

One of the DevOps engineers was trying to deploy a python app on Kubernetes cluster. Unfortunately, due to some mis-configuration, the application is not coming up. Please take a look into it and fix the issues. Application should be accessible on the specified nodePort.


1. The deployment name is `python-deployment-devops`, its using `poroko/flask-demo-appimage`. The deployment and service of this app is already deployed.

2. nodePort should be `32345` and targetPort should be python flask app's default port.


Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.


## Solution:

**1. Check existing running deployment, pods & services**

```

thor@jump_host ~$ kubectl get all
NAME                                           READY   STATUS             RESTARTS   AGE
pod/python-deployment-devops-678b746b7-tc25p   0/1     ImagePullBackOff   0          3m

NAME                            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes              ClusterIP   10.96.0.1      <none>        443/TCP          48m
service/python-service-devops   NodePort    10.96.54.156   <none>        8080:32345/TCP   3m

NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/python-deployment-devops   0/1     1            0           3m1s

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/python-deployment-devops-678b746b7   1         1         0       3m1s
thor@jump_host ~$ 
```

**2. Describe the configuration of the deployment**

```

thor@jump_host ~$ kubectl describe deploy
Name:                   python-deployment-devops
Namespace:              default
CreationTimestamp:      Wed, 11 Oct 2023 10:34:42 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=python_app
Replicas:               1 desired | 1 updated | 1 total | 0 available | 1 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=python_app
  Containers:
   python-container-devops:
    Image:        poroko/flask-app-demo
    Port:         5000/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      False   MinimumReplicasUnavailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  <none>
NewReplicaSet:   python-deployment-devops-678b746b7 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  6m2s  deployment-controller  Scaled up replica set python-deployment-devops-678b746b7 to 1
thor@jump_host ~$ 
```

**3. Refer the logs to identify the  issues** 

```

thor@jump_host ~$ kubectl logs pod/python-deployment-devops-678b746b7-tc25p
Error from server (BadRequest): container "python-container-devops" in pod "python-deployment-devops-678b746b7-tc25p" is waiting to start: trying and failing to pull image
```

**4. Edit deployment and change the image name.**

```

poroko/flask-app-demo instead of change image name poroko/flask-demo-app

thor@jump_host ~$ kubectl edit deploy
deployment.apps/python-deployment-devops edited

thor@jump_host ~$ kubectl get deploy
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
python-deployment-devops   1/1     1            1           11m
thor@jump_host ~$ 

thor@jump_host ~$ kubectl get svc
NAME                    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes              ClusterIP   10.96.0.1      <none>        443/TCP          57m
python-service-devops   NodePort    10.96.54.156   <none>        8080:32345/TCP   11m
thor@jump_host ~$ 
```

**5. Wait for deployment & pods for ready & running status**

```

thor@jump_host ~$ kubectl get pods
NAME                                        READY   STATUS    RESTARTS   AGE
python-deployment-devops-7859694dcf-26m6c   1/1     Running   0          3m14s
thor@jump_host ~$ kubectl get deploy
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
python-deployment-devops   1/1     1            1           13m
thor@jump_host ~$ 
```

**6. Search for port and change to default one seen in deployment.** 


- port: 8080 instead of change-> port: 5000

```

thor@jump_host ~$ kubectl edit svc
service/kubernetes skipped
service/python-service-devops edited


thor@jump_host ~$ kubectl get svc
NAME                    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes              ClusterIP   10.96.0.1      <none>        443/TCP          63m
python-service-devops   NodePort    10.96.54.156   <none>        5000:32345/TCP   17m
thor@jump_host ~$ 
```

**7. Validate by clicking on app icon to open page**

![py app deploy](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4de69609-2b2e-4c1c-8ee7-d738ae9d9b1b)


**8. Click on `Finish` & `Confirm` to complete the task successfully**

