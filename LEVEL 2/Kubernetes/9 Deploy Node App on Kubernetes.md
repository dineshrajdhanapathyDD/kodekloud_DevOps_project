

## Questions:

The Nautilus development team has completed development of one of the node applications, which they are planning to deploy on a Kubernetes cluster. They recently had a meeting with the DevOps team to share their requirements. Based on that, the DevOps team has listed out the exact requirements to deploy the app. Find below more details:


1. Create a deployment using `gcr.io/kodekloud/centos-ssh-enabled:node` image, replica count must be `2`.

2. Create a service to expose this app, the service type must be `NodePort`, targetPort must be `8080` and nodePort should be `30012`.

3. Make sure all the pods are in `Running` state after the deployment.

4. You can check the application by clicking on `NodeApp` button on top bar.


(You can use any labels as per your choice).


`Note`: The `kubectl` on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:  

**1. At first  kubectl  utility configure and working from jump server, run below commands**

```

thor@jump_host ~$ kubectl get namespace
NAME                 STATUS   AGE
default              Active   13m
kube-node-lease      Active   13m
kube-public          Active   13m
kube-system          Active   13m
local-path-storage   Active   13m
thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
```

**2.  Create a YAML  file with all the parameters**

```

thor@jump_host ~$ vi /tmp/node.yaml
thor@jump_host ~$ cat /tmp/node.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-deployment-xfusion
  labels:
    app: tomcat
spec:
  replicas: 2
  selector:
    matchLabels:
      app: tomcat
  template:
    metadata:
      name: tomcat
      labels:
        app: tomcat
    spec:
      containers:
        - name: tomcat-container-xfusion
          image: gcr.io/kodekloud/centos-ssh-enabled:node
          ports:
            - containerPort: 8080
          imagePullPolicy: IfNotPresent
      restartPolicy: Always
---
apiVersion: v1
kind: Service
metadata:
  name: tomcat-service-xfusion
spec:
  selector:
    app: tomcat
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
      nodePort: 30012
  type: NodePort
```

**3.  Run the below command to create a pod**

```

thor@jump_host ~$ kubectl create -f /tmp/node.yaml
deployment.apps/tomcat-deployment-xfusion created
service/tomcat-service-xfusion created
```

**4.  Wait for the pod to get completed status**

```

service:
thor@jump_host ~$ kubectl get service
NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes               ClusterIP   10.96.0.1      <none>        443/TCP          27m
tomcat-service-xfusion   NodePort    10.96.176.92   <none>        8080:30012/TCP   2m27s

pods:
thor@jump_host ~$ kubectl get pods
NAME                                         READY   STATUS    RESTARTS   AGE
tomcat-deployment-xfusion-798c5945f4-57l87   1/1     Running   0          3m2s
tomcat-deployment-xfusion-798c5945f4-pz6cj   1/1     Running   0          3m2s
```

**5.  Validate the task by open port view You can check the application by clicking on `NodeApp` button on top bar.**

if click node apps - 

https://30012-port-24bbe718ffd54b51.labs.kodekloud.com/


**6. Click on `Finish` & `Confirm` to complete the task successful**
