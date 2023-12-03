

## Questions:

The Nautilus DevOps team wants to create a `ReplicationController` to deploy several pods. They are going to deploy applications on these pods, these applications need highly available infrastructure. Below you can find exact details, create the `ReplicationController` accordingly.

1. Create a `ReplicationController` using `nginx` image, preferably with `latest` tag, and name it as `nginx-replicationcontroller`.

2. Labels `app` should be `nginx_app`, and labels `type` should be `front-end`. The container should be named as `httpd-container` and also make sure replica counts are 

3.All `pods` should be running state after deployment.

Note: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.



## Solution: 

**1. At first  check existing deployment and  pods running status**

```
thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   113s
kube-node-lease      Active   113s
kube-public          Active   113s
kube-system          Active   113s
local-path-storage   Active   100s

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create yaml  file with all the parameters , you can copy form gitlab**

```
thor@jump_host ~$ vi /tmp/replica.yml
thor@jump_host ~$ cat /tmp/replica.yml
apiVersion: v1

kind: ReplicationController

metadata:

  name: nginx-replicationcontroller

  labels:

    app: nginx_app

    type: front-end

spec:

  replicas: 3

  selector:

    app: nginx_app

  template:

    metadata:

      name: nginx_pod

      labels:

        app: nginx_app

        type: front-end

    spec:

      containers:

        - name: nginx-container

          image: nginx:latest

          ports:

            - containerPort: 80
```

**3. Run below command to create pod** 

```
thor@jump_host ~$ kubectl create -f /tmp/replica.yml
replicationcontroller/nginx-replicationcontroller created
```

**4.  Wait for  pods to get running status**

```
thor@jump_host ~$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-replicationcontroller-4dbb9   1/1     Running   0          25s
nginx-replicationcontroller-g8qs6   1/1     Running   0          25s
nginx-replicationcontroller-zpld8   1/1     Running   0          25s
```

**6.  Validate the task by running below command** 

```
thor@jump_host ~$ kubectl exec nginx-replicationcontroller-4dbb9  -- curl http://localhost
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
100   615  100   615    0     0   889k      0 --:--:-- --:--:-- --:--:--  600k
```

**7.  Click on `Finish` & `Confirm` to complete the task successful**
