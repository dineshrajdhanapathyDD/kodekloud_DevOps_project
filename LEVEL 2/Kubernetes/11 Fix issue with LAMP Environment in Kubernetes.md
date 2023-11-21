

## Questions:

One of the DevOps team member was trying to install a WordPress website on a LAMP stack which is essentially deployed on Kubernetes cluster. It was working well and we could see the installation page a few hours ago. However something is messed up with the stack now due to a website went down. Please look into the issue and fix it:

FYI, deployment name is `lamp-wp` and its using a service named `lamp-service`. The Apache is using http default port and nodeport is `30008`. From the application logs it has been identified that application is facing some issues while connecting to the database in addition to other issues. Additionally, there are some environment variables associated with the pods like `MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD, MYSQL_HOST`.

Also do not try to delete/modify any other existing components like deployment name, service name, types, labels etc.

`Note`: The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


## Solution:  

**1. Check existing running pods and deploy:**

```

thor@jump_host ~$ k get po,deploy,svc,secrets
NAME                           READY   STATUS    RESTARTS   AGE
pod/lamp-wp-56c7c454fc-l7cj8   2/2     Running   0          9m36s

NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/lamp-wp   1/1     1            1           9m36s

NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP          15m
service/lamp-service    NodePort    10.96.147.12    <none>        8080:30008/TCP   9m36s
service/mysql-service   ClusterIP   10.96.171.144   <none>        3306/TCP         9m36s

NAME                     TYPE     DATA   AGE
secret/mysql-db-url      Opaque   1      9m37s
secret/mysql-host        Opaque   1      9m37s
secret/mysql-root-pass   Opaque   1      9m38s
secret/mysql-user-pass   Opaque   2      9m37s
```

**2. From the httpd container, test connectivity to the MySQL.**

```

thor@jump_host ~$ kubectl exec -it lamp-wp-56c7c454fc-l7cj8 -c httpd-php-container -- mysql

error: Internal error occurred: error executing command in container: failed to exec in container: failed to start exec "fc05200c708bd534022e7fafa37148e03f7581b590923420ff630d956a4e5840": OCI runtime exec failed: exec failed: unable to start container process: exec: "mysql": executable file not found in $PATH: unknown
```

**3. In some iterations of the labs, the nodePort could be showing 30009 or any other port instead of 30008. If you encountered this, retrieve the YAML and modify it.**

```

thor@jump_host ~$ k get svc lamp-service -o yaml > lamp-svc.yml
thor@jump_host ~$ vi lamp-svc.yml

While inside the vi editor, type this to replace all 30009 with 30008. In addition to this, the port and targetPort might show as 8080, Change this to 80.

apiVersion: v1
kind: Service
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"app":"lamp"},"name":"lamp-service","namespace":"default"},"spec":{"ports":[{"nodePort":30008,"port":80}],"selector":{"app":"lamp","tier":"frontend"},"type":"NodePort"}}
  creationTimestamp: "2023-09-20T15:37:14Z"
  labels:
    app: lamp
  name: lamp-service
  namespace: default
  resourceVersion: "923"
  uid: 9da902a9-9f64-4da4-aa96-1e8a1781e70c
spec:
  clusterIP: 10.96.147.12
  clusterIPs:
  - 10.96.147.12
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 30008
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: lamp
    tier: frontend
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}
```
After changing, apply.
```
thor@jump_host ~$ k apply -f lamp-svc.yml 
service/lamp-service configured
```
Verify 
```
thor@jump_host ~$ k get svc
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        23m
lamp-service    NodePort    10.96.147.12    <none>        80:30008/TCP   17m
mysql-service   ClusterIP   10.96.171.144   <none>        3306/TCP       17m
```
```
thor@jump_host ~$ k get svc
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        23m
lamp-service    NodePort    10.96.147.12    <none>        80:30008/TCP   17m
mysql-service   ClusterIP   10.96.171.144   <none>        3306/TCP       17m
```

**4. To make the troubleshooting easier, we can save the container names and pod name.**

```

thor@jump_host ~$ HTTP=$(kubectl get pod  -o=jsonpath='{.items[*].spec.containers[0].name}') ; echo "HTTP: $HTTP"
HTTP: httpd-php-container

thor@jump_host ~$ MYSQL=$(kubectl get pod -o=jsonpath='{.items[*].spec.containers[1].name}')  ; echo "MYSQL: $MYSQL"
MYSQL: mysql-container

thor@jump_host ~$ 
thor@jump_host ~$ POD=$(kubectl get pods -o=jsonpath='{.items[*].metadata.name}'); echo "POD: $POD"
POD: lamp-wp-56c7c454fc-l7cj8
```

**5. we need to modify the PHP file inside the HTTP container.**

```

thor@jump_host ~$ k logs -f $POD -c $HTTP 
-> Executing /opt/docker/provision/entrypoint.d/05-permissions.sh
-> Executing /opt/docker/provision/entrypoint.d/20-php-fpm.sh
-> Executing /opt/docker/provision/entrypoint.d/20-php.sh
-> Executing /opt/docker/bin/service.d/supervisor.d//10-init.sh
2023-09-20 15:37:32,409 CRIT Set uid to user 0
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/apache.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/cron.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/dnsmasq.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/php-fpm.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/postfix.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/ssh.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/syslog.conf" during parsing
2023-09-20 15:37:32,418 INFO RPC interface 'supervisor' initialized
2023-09-20 15:37:32,418 INFO supervisord started with pid 1
2023-09-20 15:37:33,488 INFO spawned: 'syslogd' with pid 93
2023-09-20 15:37:33,491 INFO spawned: 'php-fpmd' with pid 94
2023-09-20 15:37:33,494 INFO spawned: 'apached' with pid 95
2023-09-20 15:37:33,498 INFO spawned: 'crond' with pid 96
-> Executing /opt/docker/bin/service.d/php-fpm.d//10-init.sh
Setting php-fpm user to application
2023-09-20 15:37:33,499 INFO success: php-fpmd entered RUNNING state, process has stayed up for > than 0 seconds (startsecs)
2023-09-20 15:37:33,499 INFO success: apached entered RUNNING state, process has stayed up for > than 0 seconds (startsecs)
2023-09-20 15:37:33,500 INFO success: crond entered RUNNING state, process has stayed up for > than 0 seconds (startsecs)
-> Executing /opt/docker/bin/service.d/httpd.d//10-init.sh
-> Executing /opt/docker/bin/service.d/syslog-ng.d//10-init.sh
-> Executing /opt/docker/bin/service.d/cron.d//10-init.sh
[SYSLOG] syslog-ng[93]: syslog-ng starting up; version='3.7.2'
[Wed Sep 20 15:37:33.801498 2023] [so:warn] [pid 115:tid 139842994527048] AH01574: module socache_shmcb_module is already loaded, skipping
[Wed Sep 20 15:37:33.895627 2023] [so:warn] [pid 115:tid 139842994527048] AH01574: module socache_shmcb_module is already loaded, skipping
[Wed Sep 20 15:37:33.897912 2023] [lbmethod_heartbeat:notice] [pid 115:tid 139842994527048] AH02282: No slotmem from mod_heartmonitor
[Wed Sep 20 15:37:33.993124 2023] [mpm_event:notice] [pid 115:tid 139842994527048] AH00489: Apache/2.4.25 (Unix) LibreSSL/2.4.4 configured -- resuming normal operations
[Wed Sep 20 15:37:33.993162 2023] [core:notice] [pid 115:tid 139842994527048] AH00094: Command line: '/usr/sbin/httpd -D FOREGROUND'
2023-09-20 15:37:34,590 INFO success: syslogd entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
[20-Sep-2023 15:37:34] NOTICE: fpm is running, pid 94
[20-Sep-2023 15:37:34] NOTICE: ready to handle connections
```

**6. Open a shell to the HTTP container and find the index.php file.**

```

thor@jump_host ~$ k exec -it $POD -c $HTTP -- sh
/ # ls -la
total 96
drwxr-xr-x    1 root     root          4096 Sep 20 15:37 .
drwxr-xr-x    1 root     root          4096 Sep 20 15:37 ..
srwx------    1 root     root             0 Sep 20 15:37 .supervisor.sock
drwxr-xr-x    1 applicat applicat      4096 Sep 20 15:37 app
drwxr-xr-x    1 root     root          4096 Jul  2  2017 bin
drwxr-xr-x    5 root     root           380 Sep 20 15:37 dev
lrwxrwxrwx    1 root     root            12 Sep 20 15:37 docker.stderr -> /proc/1/fd/2
lrwxrwxrwx    1 root     root            12 Sep 20 15:37 docker.stdout -> /proc/1/fd/1
lrwxrwxrwx    1 root     root            29 Jul  2  2017 entrypoint -> /opt/docker/bin/entrypoint.sh
lrwxrwxrwx    1 root     root            28 Jul  2  2017 entrypoint.cmd -> /opt/docker/bin/entrypoint.d
drwx------    2 root     root          4096 Jul  2  2017 entrypoint.d
drwxr-xr-x    1 root     root          4096 Sep 20 15:37 etc
drwxr-xr-x    1 root     root          4096 Jul  2  2017 home
drwxr-xr-x    1 root     root          4096 Jul  2  2017 lib
drwxr-xr-x    2 root     root          4096 Jul  2  2017 log
drwxr-xr-x    5 root     root          4096 Jun 25  2017 media
drwxr-xr-x    2 root     root          4096 Jun 25  2017 mnt
drwxr-xr-x    1 root     root          4096 Jul  2  2017 opt
dr-xr-xr-x 2894 root     root             0 Sep 20 15:37 proc
drwx------    1 root     root          4096 Sep 20 15:58 root
drwxr-xr-x    1 root     root          4096 Sep 20 15:37 run
drwxr-xr-x    1 root     root          4096 Jul  2  2017 sbin
drwxr-xr-x    2 root     root          4096 Jun 25  2017 srv
dr-xr-xr-x   13 nobody   nobody           0 Sep 20 15:37 sys
drwxrwxrwt    1 root     root          4096 Sep 20 15:37 tmp
drwxr-xr-x    1 root     root          4096 Jul  2  2017 usr
drwxr-xr-x    1 root     root          4096 Jul  2  2017 var
/ # ls -la app
total 12
drwxr-xr-x    1 applicat applicat      4096 Sep 20 15:37 .
drwxr-xr-x    1 root     root          4096 Sep 20 15:37 ..
-rw-r--r--    1 root     root           435 Sep 20 15:37 index.php
```

**7. Check the index.app file. Here we can see that there's some typo error. Edit this to match the variables specified in the instructions/requirements.**

```

thor@jump_host ~$ k exec -it $POD -c $HTTP -- sh
/ # cat /app/index.php 
<?php
$dbname = $_ENV['MYSQL_DATABASE'];
$dbuser = $_ENV['MYSQL_USER'];
$dbpass = $_ENV['MYSQL_PASSWORDS'];
$dbhost = $_ENV['HOST_MYQSL'];

$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}

Change MYSQL_PASSWORDS -> MYSQL_PASSWORD
Change HOST_MYSQL -> MYSQL_HOST

/ # vi /app/index.php 
/ # cat /app/index.php 
<?php
$dbname = $_ENV['MYSQL_DATABASE'];
$dbuser = $_ENV['MYSQL_USER'];
$dbpass = $_ENV['MYSQL_PASSWORD'];
$dbhost = $_ENV['MYSQL_HOST'];


$connect = mysqli_connect($dbhost, $dbuser, $dbpass) or die("Unable to Connect to '$dbhost'");

$test_query = "SHOW TABLES FROM $dbname";
$result = mysqli_query($test_query);

if ($result->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
  echo "Connected successfully";/ 
```

**8. While inside the container, restart the php-fpm service. Afterwards, exit out of the container.**

# service php-fpm restart
php-fpm:php-fpmd: stopped
php-fpm:php-fpmd: started
/ # service php-fpm status
php-fpm:php-fpmd                 RUNNING   pid 269, uptime 0:00:12


**9. Checking the logs again:**

```

thor@jump_host ~$ k logs -f $POD -c $HTTP
-> Executing /opt/docker/provision/entrypoint.d/05-permissions.sh
-> Executing /opt/docker/provision/entrypoint.d/20-php-fpm.sh
-> Executing /opt/docker/provision/entrypoint.d/20-php.sh
-> Executing /opt/docker/bin/service.d/supervisor.d//10-init.sh
2023-09-20 15:37:32,409 CRIT Set uid to user 0
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/apache.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/cron.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/dnsmasq.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/php-fpm.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/postfix.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/ssh.conf" during parsing
2023-09-20 15:37:32,409 WARN Included extra file "/opt/docker/etc/supervisor.d/syslog.conf" during parsing
2023-09-20 15:37:32,418 INFO RPC interface 'supervisor' initialized
2023-09-20 15:37:32,418 INFO supervisord started with pid 1
2023-09-20 15:37:33,488 INFO spawned: 'syslogd' with pid 93
2023-09-20 15:37:33,491 INFO spawned: 'php-fpmd' with pid 94
2023-09-20 15:37:33,494 INFO spawned: 'apached' with pid 95
2023-09-20 15:37:33,498 INFO spawned: 'crond' with pid 96
-> Executing /opt/docker/bin/service.d/php-fpm.d//10-init.sh
Setting php-fpm user to application
2023-09-20 15:37:33,499 INFO success: php-fpmd entered RUNNING state, process has stayed up for > than 0 seconds (startsecs)
2023-09-20 15:37:33,499 INFO success: apached entered RUNNING state, process has stayed up for > than 0 seconds (startsecs)
2023-09-20 15:37:33,500 INFO success: crond entered RUNNING state, process has stayed up for > than 0 seconds (startsecs)
-> Executing /opt/docker/bin/service.d/httpd.d//10-init.sh
-> Executing /opt/docker/bin/service.d/syslog-ng.d//10-init.sh
-> Executing /opt/docker/bin/service.d/cron.d//10-init.sh
[SYSLOG] syslog-ng[93]: syslog-ng starting up; version='3.7.2'
[Wed Sep 20 15:37:33.801498 2023] [so:warn] [pid 115:tid 139842994527048] AH01574: module socache_shmcb_module is already loaded, skipping
[Wed Sep 20 15:37:33.895627 2023] [so:warn] [pid 115:tid 139842994527048] AH01574: module socache_shmcb_module is already loaded, skipping
[Wed Sep 20 15:37:33.897912 2023] [lbmethod_heartbeat:notice] [pid 115:tid 139842994527048] AH02282: No slotmem from mod_heartmonitor
[Wed Sep 20 15:37:33.993124 2023] [mpm_event:notice] [pid 115:tid 139842994527048] AH00489: Apache/2.4.25 (Unix) LibreSSL/2.4.4 configured -- resuming normal operations
[Wed Sep 20 15:37:33.993162 2023] [core:notice] [pid 115:tid 139842994527048] AH00094: Command line: '/usr/sbin/httpd -D FOREGROUND'
2023-09-20 15:37:34,590 INFO success: syslogd entered RUNNING state, process has stayed up for > than 1 seconds (startsecs)
[20-Sep-2023 15:37:34] NOTICE: fpm is running, pid 94
[20-Sep-2023 15:37:34] NOTICE: ready to handle connections
[20-Sep-2023 16:06:50] NOTICE: Terminating ...
2023-09-20 16:06:50,403 INFO waiting for php-fpmd to stop
[20-Sep-2023 16:06:50] NOTICE: exiting, bye-bye!
2023-09-20 16:06:50,417 INFO stopped: php-fpmd (exit status 0)
2023-09-20 16:06:50,422 INFO spawned: 'php-fpmd' with pid 269
2023-09-20 16:06:50,422 INFO success: php-fpmd entered RUNNING state, process has stayed up for > than 0 seconds (startsecs)
-> Executing /opt/docker/bin/service.d/php-fpm.d//10-init.sh
Setting php-fpm user to application
[20-Sep-2023 16:06:50] NOTICE: fpm is running, pid 269
[20-Sep-2023 16:06:50] NOTICE: ready to handle connections
```

**10. Check the environment variables in both containers.**

```

thor@jump_host ~$ kubectl exec -it $POD -c $HTTP -- env | grep -i mysql
MYSQL_ROOT_PASSWORD=R00t
MYSQL_DATABASE=kodekloud_db2
MYSQL_USER=kodekloud_gem
MYSQL_PASSWORD=Rc5C9EyvbU
MYSQL_HOST=mysql-service
MYSQL_SERVICE_SERVICE_PORT=3306
MYSQL_SERVICE_PORT=tcp://10.96.171.144:3306
MYSQL_SERVICE_PORT_3306_TCP=tcp://10.96.171.144:3306
MYSQL_SERVICE_PORT_3306_TCP_PROTO=tcp
MYSQL_SERVICE_PORT_3306_TCP_PORT=3306
MYSQL_SERVICE_SERVICE_HOST=10.96.171.144
MYSQL_SERVICE_PORT_3306_TCP_ADDR=10.96.171.144


thor@jump_host ~$ kubectl exec -it $POD -c $MYSQL -- env | grep -i mysql
MYSQL_MAJOR=5.6
MYSQL_VERSION=5.6.51-1debian9
MYSQL_ROOT_PASSWORD=R00t
MYSQL_DATABASE=kodekloud_db2
MYSQL_USER=kodekloud_gem
MYSQL_PASSWORD=Rc5C9EyvbU
MYSQL_HOST=mysql-service
MYSQL_SERVICE_PORT=tcp://10.96.171.144:3306
MYSQL_SERVICE_PORT_3306_TCP_ADDR=10.96.171.144
MYSQL_SERVICE_SERVICE_HOST=10.96.171.144
MYSQL_SERVICE_SERVICE_PORT=3306
MYSQL_SERVICE_PORT_3306_TCP=tcp://10.96.171.144:3306
MYSQL_SERVICE_PORT_3306_TCP_PROTO=tcp
MYSQL_SERVICE_PORT_3306_TCP_PORT=3306


thor@jump_host ~$ kubectl describe pod | grep -A 6 Env
    Environment:
      MYSQL_ROOT_PASSWORD:  <set to the key 'password' in secret 'mysql-root-pass'>  Optional: false
      MYSQL_DATABASE:       <set to the key 'database' in secret 'mysql-db-url'>     Optional: false
      MYSQL_USER:           <set to the key 'username' in secret 'mysql-user-pass'>  Optional: false
      MYSQL_PASSWORD:       <set to the key 'password' in secret 'mysql-user-pass'>  Optional: false
      MYSQL_HOST:           <set to the key 'host' in secret 'mysql-host'>           Optional: false
    Mounts:
--
    Environment:
      MYSQL_ROOT_PASSWORD:  <set to the key 'password' in secret 'mysql-root-pass'>  Optional: false
      MYSQL_DATABASE:       <set to the key 'database' in secret 'mysql-db-url'>     Optional: false
      MYSQL_USER:           <set to the key 'username' in secret 'mysql-user-pass'>  Optional: false
      MYSQL_PASSWORD:       <set to the key 'password' in secret 'mysql-user-pass'>  Optional: false
      MYSQL_HOST:           <set to the key 'host' in secret 'mysql-host'>           Optional: false
    Mounts:
```

**11. Click the App button at the top right to open the app URL in the new tab.**

`https://30008-port-8fe4df238fd44458.labs.kodekloud.com/`

-  Connected successfully


**12. Click on `Finish` & `Confirm` to complete the task successful**
