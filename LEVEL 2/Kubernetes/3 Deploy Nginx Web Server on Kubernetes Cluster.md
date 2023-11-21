

## Questions:

Some of the Nautilus team developers are developing a static website and they want to deploy it on Kubernetes cluster. They want it to be highly available and scalable. Therefore, based on the requirements, the DevOps team has decided to create a deployment for it with multiple replicas. Below you can find more details about it:



1. Create a deployment using nginx image with latest tag only and remember to mention the tag i.e nginx:latest. Name it as nginx-deployment. The container should be named as nginx-container, also make sure replica counts are 3.

2. Create a NodePort type service named nginx-service. The nodePort should be 30011.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.

## Solution:  

**1. At first  check existing deployment and  pods running status**  

```

thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   3h28m
kube-node-lease      Active   3h28m
kube-public          Active   3h28m
kube-system          Active   3h28m
local-path-storage   Active   3h28m
thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create YAML  file with all the parameters**

```

thor@jump_host ~$ vi /tmp/nginx.yml
thor@jump_host ~$ cat /tmp/nginx.yml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx-app
    type: front-end
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30011
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-app
    type: front-end
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-app
      type: front-end
  template:
    metadata:
      labels:
        app: nginx-app
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
```
 
**3. Run below command to create pod**

```

thor@jump_host ~$ kubectl create -f /tmp/nginx.yml
service/nginx-service created
deployment.apps/nginx-deployment created
```

**4.  Wait for pods to get running status**

```

thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-56cbd5d774-5t9wm   1/1     Running   0          31s
nginx-deployment-56cbd5d774-m7zjq   1/1     Running   0          31s
nginx-deployment-56cbd5d774-qtzwk   1/1     Running   0          31s
```

**5.  Validate the task by running below command**

```

thor@jump_host ~$ kubectl exec nginx-deployment-56cbd5d774-m7zjq -- curl http://localhost
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
100   615  100   615    0     0   709k      0 --:--:-- --:--:-- --:--:--  600k
```

**6.  Click on `Finish` & `Confirm` to complete the task successfully**


