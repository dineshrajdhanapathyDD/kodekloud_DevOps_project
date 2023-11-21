

## Questions:

A new java-based application is ready to be deployed on a Kubernetes cluster. The development team had a meeting with the DevOps team to share the requirements and application scope. The team is ready to setup an application stack for it under their existing cluster. Below you can find the details for this:



- Create a namespace named tomcat-namespace-devops.

-  Create a deployment for tomcat app which should be named as tomcat-deployment-devops under the same namespace you created. Replica count should be 1, the container should be named as tomcat-container-devops, its image should be gcr.io/kodekloud/centos-ssh-enabled:tomcat and its container port should be 8080.

- Create a service for tomcat app which should be named as tomcat-service-devops under the same namespace you created. Service type should be NodePort and nodePort should be 32227.

-  Before clicking on Check button please make sure the application is up and running.

You can use any labels as per your choice.

Note: The kubectl on jump_host has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```

thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   44m
kube-node-lease      Active   44m
kube-public          Active   44m
kube-system          Active   44m
local-path-storage   Active   43m

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2. Create new namespace** 

```

thor@jump_host ~$ kubectl create namespace tomcat-namespace-devops
namespace/tomcat-namespace-nautilus created

thor@jump_host ~$ kubectl get namespace
NAME                        STATUS   AGE
default                     Active   46m
kube-node-lease             Active   46m
kube-public                 Active   46m
kube-system                 Active   46m
local-path-storage          Active   46m
tomcat-namespace-nautilus   Active   26s
```

**3.  Create yaml  file with all the parameters**


```

thor@jump_host ~$ vi /tmp/tomcat.yaml

apiVersion: v1
kind: Service
metadata:
  name: tomcat-service-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  type: NodePort
  selector:
    app: tomcat
  ports:
    - port: 80
      protocol: TCP
      targetPort: 8080
      nodePort: 32227
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-deployment-nautilus
  namespace: tomcat-namespace-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat
  template:
    metadata:
      labels:
        app: tomcat
    spec:
      containers:
        - name: tomcat-container-xfusion
          image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
          ports:
            - containerPort: 8080

exit  :wq

thor@jump_host ~$ cat /tmp/tomcat.yaml
```

**4. Run below command to create pod**

```

thor@jump_host ~$ kubectl create -f /tmp/tomcat.yaml
service/tomcat-service-devops created
deployment.apps/tomcat-deployment-devops created

thor@jump_host ~$ kubectl get deploy -n tomcat-namespace-devops
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
tomcat-deployment-devops   1/1     1            1           34s

thor@jump_host ~$ kubectl get pods -n tomcat-namespace-devops
NAME                                        READY   STATUS    RESTARTS   AGE
tomcat-deployment-devops-67846cfffd-9v57f   1/1     Running   0          63s
```

**6.  Validate the task by running below command**

```

thor@jump_host ~$ kubectl exec tomcat-deployment-devops-67846cfffd-9v57f -n tomcat-namespace-devops -- curl http://localhost:8080
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->
<html>
    <head>
        <title>SampleWebApp</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
    </head>
    <body>
        <h2>Welcome to xFusionCorp Industries!</h2>
        <br>
    
    </body>
</html>
100   471  100   471    0     0   2378      0 --:--:-- --:--:-- --:--:--  2378
```

**7.  Click on `Finish` & `Confirm` to complete the task successful**


