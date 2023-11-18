

## Questions:

As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file `/opt/docker/Dockerfile` `please keep (D) capital of Dockerfile` on `App server 1` in `Stratos DC` and configure to build an image with the following requirements:

a. Use `ubuntu` as the base image.

b. Install `apache2` and configure it to work on `5001` port. `do not update any other Apache configuration settings like document root etc`.


## Solution:  


**1. At first login on app server as per the task - app server -1& switch to root user**

```

thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:8UY76S07dW8a978eW/+X4vjA+MdLnyqe7QcR4J4GWyY.
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
```

**2. Run the Below command to check existing docker images**

```

[root@stapp01 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@stapp01 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

**3. Write a  Vi Dockerfile  in folder /opt/docker**

```

[root@stapp01 ~]# vi /opt/docker/Dockerfile
[root@stapp01 ~]# cat /opt/docker/Dockerfile
FROM ubuntu

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update

RUN apt-get install apache2 -y

RUN sed -i "s/80/5001/g" /etc/apache2/ports.conf

EXPOSE 5001

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND", "-k", "start"]
```

**4. Run Build command to  create an image**

```

root@stapp01 ~]# cd /opt/docker/
[root@stapp01 docker]# docker build  -t apache2 .
Sending build context to Docker daemon  2.048kB
Step 1/7 : FROM ubuntu
 ---> c6b84b685f35
Step 2/7 : ARG DEBIAN_FRONTEND=noninteractive
 ---> Using cache
 ---> 2f82b2edbb1a
Step 3/7 : RUN apt-get update
 ---> Using cache
 ---> 2b291a279fcf
Step 4/7 : RUN apt-get install apache2 -y
 ---> Using cache
 ---> e7eecf409913
Step 5/7 : RUN sed -i "s/80/5001/g" /etc/apache2/ports.conf
 ---> Running in 64034591b627
Removing intermediate container 64034591b627
 ---> 0b78193276d4
Step 6/7 : EXPOSE 5001
 ---> Running in a7bbe04a504f
Removing intermediate container a7bbe04a504f
 ---> 8c6ba5bd33e2
Step 7/7 : CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND", "-k", "start"]
 ---> Running in 59d239b9da86
Removing intermediate container 59d239b9da86
 ---> e900b758d4e3
Successfully built e900b758d4e3
Successfully tagged apache2:latest
```

**5. Run the Below command to  run the docker container on the server**

```
[root@stapp01 docker]# docker run --name http -p 5001:5001 -d apache2
32329399b32a0f954685cb8bebf53fba6886277e6ffe7cc5a8ad3fca4f587e63
```

**6.  Validate the task by docker ps  &   curl**

```

[root@stapp01 docker]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
32329399b32a        apache2             "/usr/sbin/apache2ct…"   46 seconds ago      Up 44 seconds       0.0.0.0:5001->5001/tcp   http


[root@stapp01 docker]# curl -Ik http://localhost:5001
HTTP/1.1 200 OK
Date: Tue, 12 Sep 2023 10:36:30 GMT
Server: Apache/2.4.52 (Ubuntu)
Last-Modified: Tue, 12 Sep 2023 10:26:31 GMT
ETag: "29af-60526e057a3c0"
Accept-Ranges: bytes
Content-Length: 10671
Vary: Accept-Encoding
Content-Type: text/html
```

**7.  Click on `Finish` & `Confirm` to complete the task successfully**


