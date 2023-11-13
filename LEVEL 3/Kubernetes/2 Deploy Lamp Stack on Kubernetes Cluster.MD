


## Questions:

The Nautilus DevOps team want to deploy a PHP website on Kubernetes cluster. They are going to use Apache as a web server and Mysql for database. The team had already gathered the requirements and now they want to make this website live. Below you can find more details:

1) Create a config map `php-config` for `php.ini` with `variables_order = "EGPCS"` data.

2) Create a deployment named `lamp-wp`.

3) Create two containers under it. First container must be `httpd-php-container` using image `webdevops/php-apache:alpine-3-php7` and second `container` must be mysql-container from image `mysql:5.6`. Mount `php-config` configmap in httpd container at `/opt/docker/etc/php/php.ini` location.

4) Create kubernetes generic secrets for mysql related values like myql root password, mysql user, mysql password, mysql host and mysql database. Set any values of your choice.


5) Add some environment variables for both containers:


a) `MYSQL_ROOT_PASSWORD`, `MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD` and `MYSQL_HOST`. Take their values from the secrets you created. Please make sure to use `env` field (do not use `envFrom`) to define the name-value pair of environment variables.


6) Create a node port type service `lamp-service` to expose the web application, nodePort must be `30008`.


7) Create a service for mysql named `mysql-service` and its port must be `3306`.


8) We already have `/tmp/index.php` file on `jump_host` server.


a) Copy this file into httpd container under Apache document root i.e `/app` and replace the dummy values for mysql related variables with the environment variables you have set for mysql related parameters. Please make sure you do not hard code the mysql related details in this file, you must use the environment variables to fetch those values.


b) You must be able to access this `index.php` on node port `30008` at the end, please note that you should see `Connected successfully` message while accessing this page.


`Note`:

The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:

**1. Check existing running Pods  & Services**

```

thor@jump_host ~$ kubectl get pods
No resources found in default namespace.

thor@jump_host ~$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   55m
thor@jump_host ~$ 
```

**2.  Create a  Secret  with all the parameters**

```

thor@jump_host ~$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   61m

thor@jump_host ~$ kubectl get secrets
NAME       TYPE     DATA   AGE
database   Opaque   5      96s

thor@jump_host ~$ kubectl create secret generic database --from-literal=MYSQL_ROOT_PASSWORD=nbtech --from-literal=MYSQL_DATABASE=kodekloud --from-literal=MYSQL_USER=nbtechsupport --from-literal=MYSQL_PASSWORD=nbtech --from-literal=MYSQL_HOST=mysql-service
secret/database created

thor@jump_host ~$ kubectl get secrets
NAME       TYPE     DATA   AGE
database   Opaque   5      96s
```

**3. create a YAML  file with all parameters**
 
```

thor@jump_host ~$ vi /tmp/lamp.yaml
thor@jump_host ~$ cat /tmp/lamp.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  creationTimestamp: null
  name: php-config
data:
  php.ini: |
     variables_order = "EGPCS"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: lamp-wp
  name: lamp-wp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: lamp-wp
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: lamp-wp
    spec:
      containers:
      - image: webdevops/php-apache:alpine-3-php7
        name: httpd-php-container
        resources: {}
        ports:
        - containerPort: 80
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_ROOT_PASSWORD
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_PASSWORD
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_DATABASE
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_USER
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_HOST
        volumeMounts:
        - name: php-ini
          mountPath: /opt/docker/etc/php/php.ini
          subPath: php.ini
      - image: mysql:5.6
        name: mysql-container
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_ROOT_PASSWORD
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_PASSWORD
        - name: MYSQL_DATABASE
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_DATABASE
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_USER
        - name: MYSQL_HOST
          valueFrom:
            secretKeyRef:
              name: database
              key: MYSQL_HOST
        ports:
        - containerPort: 3306
        resources: {}
      volumes:
      - name: php-ini
        configMap:
          name: php-config
status: {}


---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
spec:
  selector:
    app: lamp-wp
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
---
apiVersion: v1
kind: Service
metadata:
  name: lamp-service
spec:
  type: NodePort
  selector:
    app: lamp-wp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30008

thor@jump_host ~$ 

thor@jump_host ~$ kubectl create -f /tmp/lamp.yaml
configmap/php-config created
deployment.apps/lamp-wp created
service/mysql-service created
service/lamp-service created
thor@jump_host ~$ 
```

**4. Wait for deployment and pods running status**

```

thor@jump_host ~$ kubectl get all
NAME                           READY   STATUS    RESTARTS   AGE
pod/lamp-wp-6d66967b4f-w9p5m   2/2     Running   0          44s

NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        69m
service/lamp-service    NodePort    10.96.27.2      <none>        80:30008/TCP   43s
service/mysql-service   ClusterIP   10.96.235.196   <none>        3306/TCP       44s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/lamp-wp   1/1     1            1           44s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/lamp-wp-6d66967b4f   1         1         1       44s
thor@jump_host ~$ 
```

**5. For index.php edit the jump server then copy the file on container.**

```

thor@jump_host ~$ sudo vi /tmp/index.php

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for thor: mjolnir123

thor@jump_host ~$ cat /tmp/index.php
<?php
$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];

$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
  echo "Connected successfully";


thor@jump_host ~$ kubectl get all
NAME                           READY   STATUS    RESTARTS   AGE
pod/lamp-wp-6d66967b4f-w9p5m   2/2     Running   0          9m8s

NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        77m
service/lamp-service    NodePort    10.96.27.2      <none>        80:30008/TCP   9m7s
service/mysql-service   ClusterIP   10.96.235.196   <none>        3306/TCP       9m8s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/lamp-wp   1/1     1            1           9m8s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/lamp-wp-6d66967b4f   1         1         1       9m8s


thor@jump_host ~$ kubectl get pods
NAME                       READY   STATUS    RESTARTS   AGE
lamp-wp-6d66967b4f-w9p5m   2/2     Running   0          9m53s
```

**6. copy and execute the command in a container**

```

thor@jump_host ~$ kubectl cp /tmp/index.php lamp-wp-6d66967b4f-w9p5m:/app -c httpd-php-container
thor@jump_host ~$ kubectl exec -it lamp-wp-6d66967b4f-w9p5m -c  httpd-php-container -- cat /app/index.php
<?php
$dbname = $_ENV["MYSQL_DATABASE"];
$dbuser = $_ENV["MYSQL_USER"];
$dbpass = $_ENV["MYSQL_PASSWORD"];
$dbhost = $_ENV["MYSQL_HOST"];

$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
  echo "Connected successfully";
```

**7. Click on `Finish` & `Confirm` to complete the task successfully**
