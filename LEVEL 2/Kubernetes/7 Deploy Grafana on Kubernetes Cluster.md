

## Questions:

The Nautilus DevOps teams is planning to set up a Grafana tool to collect and analyze analytics from some applications. They are planning to deploy it on Kubernetes cluster. Below you can find more details.



1. Create a deployment named `grafana-deployment-xfusion` using any grafana image for Grafana app. Set other parameters as per your choice.


2. Create `NodePort` type service with nodePort `32000` to expose the app.


`You need not to make any configuration changes inside the Grafana app once deployed, just make sure you are able to access the Grafana login page`.


`Note`: The `kubectl` on `jump_host` has been configured to work with kubernetes cluster.


## Solution:  

**1. Check existing running Pods  & Services**

```

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.
thor@jump_host ~$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   27m
```

**2.  Create a YAML  file with all the parameters,  Kindly do the changes as per task**


```

thor@jump_host ~$ vi /tmp/grafana.yaml
thor@jump_host ~$ cat /tmp/grafana.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      name: grafana
      labels:
        app: grafana
    spec:
      securityContext:
        fsGroup: 472
        supplementalGroups:
          - 0
      containers:
        - name: grafana
          image: grafana/grafana:latest
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 3000
              name: http-grafana
              protocol: TCP
          readinessProbe:
            failureThreshold: 3
            httpGet:
              path: /robots.txt
              port: 3000
              scheme: HTTP
            initialDelaySeconds: 10
            periodSeconds: 30
            successThreshold: 1
            timeoutSeconds: 2
          livenessProbe:
            failureThreshold: 3
            initialDelaySeconds: 30
            periodSeconds: 10
            successThreshold: 1
            tcpSocket:
              port: 3000
            timeoutSeconds: 1
          resources:
            requests:
              cpu: 250m
              memory: 750Mi
---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  selector:
    app: grafana
  ports:
    - protocol: TCP
      port: 3000
      targetPort: http-grafana
      nodePort: 32000
  type: NodePort
```

**3. Run the below command to create a pod**

```

thor@jump_host ~$ kubectl create -f /tmp/grafana.yaml
deployment.apps/grafana-deployment-xfusion created
service/grafana-service created
```

**4.  Wait for the pod to get completed status**

```

thor@jump_host ~$ kubectl get service
NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
grafana-service   NodePort    10.96.204.14   <none>        3000:32000/TCP   31s
kubernetes        ClusterIP   10.96.0.1      <none>        443/TCP          32m


thor@jump_host ~$ kubectl get pods
NAME                                        READY   STATUS    RESTARTS   AGE
grafana-deployment-xfusion-79bfff94-7mdxq   0/1     Running   0          55s
thor@jump_host ~$ kubectl get pods
NAME                                        READY   STATUS    RESTARTS   AGE
grafana-deployment-xfusion-79bfff94-7mdxq   1/1     Running   0          2m53s
```

**5.  Validate the task by open port view WUI**


- click on Grafana from top bar to open url:

Grafana app once deployed, just make sure you are able to access the Grafana login page.


**6.  Click on `Finish` & `Confirm` to complete the task successfully.**

