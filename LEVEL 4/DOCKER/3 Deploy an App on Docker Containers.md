


## Question 

The Nautilus Application development team recently finished development of one of the apps that they want to deploy on a containerized platform. The Nautilus Application development and DevOps teams met to discuss some of the basic pre-requisites and requirements to complete the deployment. The team wants to test the deployment on one of the app servers before going live and set up a complete containerized stack using a docker compose fie. Below are the details of the task:  
  

  

1.  On  `App Server 1`  in  `Stratos Datacenter`  create a docker compose file  `/opt/sysops/docker-compose.yml`  (should be named exactly).  
      
    
2.  The compose should deploy two services (web and DB), and each service should deploy a container as per details below:  
      
    

`For web service:`  
  

a. Container name must be  `php_apache`.  
  

b. Use image  `php`  with any  `apache`  tag. Check  [here](https://hub.docker.com/_/php?tab=tags/)  for more details.  
  

c. Map  `php_apache`  container's port  `80`  with host port  `5003`  
  

d. Map  `php_apache`  container's  `/var/www/html`  volume with host volume  `/var/www/html`.  
  

`For DB service:`  
  

a. Container name must be  `mysql_apache`.  
  

b. Use image  `mariadb`  with any tag (preferably  `latest`). Check  [here](https://hub.docker.com/_/mariadb?tab=tags/)  for more details.  
  

c. Map  `mysql_apache`  container's port  `3306`  with host port  `3306`  
  

d. Map  `mysql_apache`  container's  `/var/lib/mysql`  volume with host volume  `/var/lib/mysql`.  
  

e. Set MYSQL_DATABASE=`database_apache`  and use any custom user ( except root ) with some complex password for DB connections.  
  

3.  After running docker-compose up you can access the app with curl command  `curl <server-ip or hostname>:5003/`  
      
    

For more details check  [here](https://hub.docker.com/_/mariadb?tab=description/).  
  

`Note:`  Once you click on  `FINISH`  button, all currently running/stopped containers will be destroyed and stack will be deployed again using your compose file.

## Solution

**1. At first login on app server as per the task & Switch to root user**

```
thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:fyTSZi/3//Sm4QSLEHVjyq8BsXgy4UZCvAjm9wZyV34.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01,172.16.238.10' (ECDSA) to the list of known hosts.
tony@stapp01's password: Ir0nM@n
[tony@stapp01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: Ir0nM@n
[root@stapp01 ~]# 
```
**2. create a docker compose file  `/opt/sysops/docker-compose.yml`  (should be named exactly).**
```
[root@stapp01 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@stapp01 ~]# cd /opt/sysops
[root@stapp01 sysops]# ls
[root@stapp01 sysops]# ls -l
total 0

[root@stapp01 sysops]# pwd
/opt/sysops
[root@stapp01 sysops]# vi docker-compose.yml
[root@stapp01 sysops]# cat docker-compose.yml
version: '2'
services:
  web:
    container_name: php_apache
    image: php:apache-bullseye
    ports:
      - "5003:80"
    volumes:
      - /var/www/html:/var/www/html
  db:
    image: mariadb
    container_name: mysql_apache
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=database_apache
      - MARIADB_ROOT_PASSWORD=p4$$W0rD
[root@stapp01 sysops]# 
```

**3. Create file  compose up**

```
[root@stapp01 sysops]# docker-compose up -d
Creating network "sysops_default" with the default driver
Pulling web (php:apache-bullseye)...
apache-bullseye: Pulling from library/php
e67fdae35593: Pull complete
22a19ba793cb: Pull complete
46e419350351: Pull complete
8de997c28946: Pull complete
1d7540f99b47: Pull complete
12a700ba0368: Pull complete
9b19829d9fc5: Pull complete
e2b1e9312bb3: Pull complete
252836d61768: Pull complete
96925b1b4c84: Pull complete
692f773b58cf: Pull complete
b83c2b06c662: Pull complete
30038b2a591c: Pull complete
Pulling db (mariadb:)...
latest: Pulling from library/mariadb
43f89b94cd7d: Pull complete
dfb413a01c7e: Pull complete
1d3f76b535d3: Pull complete
f7efd05ec01e: Pull complete
fe2ff83c75df: Pull complete
50ee0c078c93: Pull complete
6975e72928bb: Pull complete
561d1b426cbd: Pull complete
Creating php_apache   ... done
Creating mysql_apache ... done
```
**4. File save and run to build**

```
[root@stapp01 sysops]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                    NAMES
eb1d21020dfb   php:apache-bullseye   "docker-php-entrypoi…"   29 seconds ago   Up 7 seconds    0.0.0.0:5003->80/tcp     php_apache
e2cbab69e93e   mariadb               "docker-entrypoint.s…"   29 seconds ago   Up 17 seconds   0.0.0.0:3306->3306/tcp   mysql_apache
[root@stapp01 sysops]# 
```

**5. Access the app with curl command**
```
[root@stapp01 sysops]# curl localhost:5003
<html>
    <head>
        <title>Welcome to xFusionCorp Industries!</title>
    </head>

    <body>
        Welcome to xFusionCorp Industries!    </body>
</html>[root@stapp01 sysops]# 
```

**6.  Click on  `Finish`  &  `Confirm`  to complete the task successful**

![2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3ecc9f11-ee74-4d94-8f8a-87b3fbdf302a)

