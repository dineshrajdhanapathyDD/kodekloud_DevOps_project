
## Questions


The Nautilus DevOps team is working to create new images per requirements shared by the development team. One of the team members is working to create a  `Dockerfile`  on  `App Server 1`  in  `Stratos DC`. While working on it she ran into issues in which the docker build is failing and displaying errors. Look into the issue and fix it to build an image as per details mentioned below:

  

a. The  `Dockerfile`  is placed on  `App Server 1`  under  `/opt/docker`  directory.  
  

b. Fix the issues with this file and make sure it is able to build the image.  
  

c. Do not change base image, any other valid configuration within Dockerfile, or any of the data been used — for example, index.html.  
  

`Note:`  Please note that once you click on  `FINISH`  button all existing images, the containers will be destroyed and new image will be built from your  `Dockerfile`.

## Solution

**1. Login on app server as per the task & Switch to root user**

```
thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:navFtI7zgy4l7UrurTsUnNfplLtSU2uWIlDeroH/UxQ.
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

**2. Run the below command to check existing docker images**

```
[root@stapp01 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
[root@stapp01 ~]# 
```

3. Edit vi **Dockerfile** under  **/opt/docker**  directory
- Error
```
IMAGE httpd:2.4.43

ADD sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

ADD sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf

ADD sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf

ADD sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf

COPY certs/server.crt /usr/local/apache2/conf/server.crt

COPY certs/server.key /usr/local/apache2/conf/server.key

COPY html/index.html /usr/local/apache2/htdocs/
```
```
[root@stapp01 ~]# cd /opt/docker
[root@stapp01 docker]# vi Dockerfile
[root@stapp01 docker]# cat Dockerfile
FROM httpd:2.4.43

RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf

RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf

RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf

COPY certs/server.crt /usr/local/apache2/conf/server.crt

COPY certs/server.key /usr/local/apache2/conf/server.key

COPY html/index.html /usr/local/apache2/htdocs/
[root@stapp01 docker]# 
```
**4. Build an image using this Dockerfile**

```
[root@stapp01 docker]# docker build -t httpd_image .
Sending build context to Docker daemon  9.216kB
Step 1/8 : FROM httpd:2.4.43
2.4.43: Pulling from library/httpd
bf5952930446: Pull complete 
3d3fecf6569b: Pull complete 
b5fc3125d912: Pull complete 
3c61041685c0: Pull complete 
34b7e9053f76: Pull complete 
Digest: sha256:cd88fee4eab37f0d8cd04b06ef97285ca981c27b4d685f0321e65c5d4fd49357
Status: Downloaded newer image for httpd:2.4.43
 ---> f1455599cc2e
Step 2/8 : RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf
 ---> Running in 57014594c251
Removing intermediate container 57014594c251
 ---> 86003c3f681a
Step 3/8 : RUN sed -i '/LoadModule\ ssl_module modules\/mod_ssl.so/s/^#//g' conf/httpd.conf
 ---> Running in 7034058ea08d
Removing intermediate container 7034058ea08d
 ---> b23570484cad
Step 4/8 : RUN sed -i '/LoadModule\ socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' conf/httpd.conf
 ---> Running in f34cef66ab9a
Removing intermediate container f34cef66ab9a
 ---> 41d1f3ce1dcc
Step 5/8 : RUN sed -i '/Include\ conf\/extra\/httpd-ssl.conf/s/^#//g' conf/httpd.conf
 ---> Running in 7f1a71f8129e
Removing intermediate container 7f1a71f8129e
 ---> 27b11cc376ea
Step 6/8 : COPY certs/server.crt /usr/local/apache2/conf/server.crt
 ---> cc312dbaefd3
Step 7/8 : COPY certs/server.key /usr/local/apache2/conf/server.key
 ---> 189f6b2816ff
Step 8/8 : COPY html/index.html /usr/local/apache2/htdocs/
 ---> 34a997f03f90
Successfully built 34a997f03f90
Successfully tagged httpd_image:latest
[root@stapp01 docker]# 
```

**5. Validate the task by docker images & run the container**

```
[root@stapp01 docker]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
httpd_image         latest              34a997f03f90        56 seconds ago      166MB
httpd               2.4.43              f1455599cc2e        3 years ago         166MB
[root@stapp01 docker]#
```

![111111](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/a3392d5f-1d39-4ad6-b39f-ec0b71836b99)

![docke complee](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4e143f09-c647-48cf-ae8e-92f4cbb5335a)

6. **Click on **Finish** & **Confirm** to complete the task successful**