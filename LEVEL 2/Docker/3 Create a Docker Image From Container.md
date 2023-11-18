

## Questions:

One of the Nautilus developer was working to test new changes on a container. He wants to keep a backup of his changes to the container. A new request has been raised for the DevOps team to create a new image from this container. Below are more details about it:


a. Create an image `cluster:devops` on `Application Server 1` from a container `ubuntu_latest` that is running on same server.


## Solution:  


**1. Login on app server 1 & Switch to  root user**

```

thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:+rF24JCr6d0F7ReZHXMAXu64KxftV1ULZUGdN2GAfzo.
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

**2. Check existing docker container ubuntu_latest running status** 

```

[root@stapp01 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
0cbf6b3b4852        ubuntu              "/bin/bash"         3 minutes ago       Up 3 minutes                            ubuntu_latest
```

**3. Look at the list of images on your system**

```

[root@stapp01 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              c6b84b685f35        3 weeks ago         77.8MB
```

**4. Create a new image from this container as per the task** 

```
[root@stapp01 ~]# docker commit ubuntu_latest cluster:devops
sha256:8eadaab68109c5f3308082c4c4337a6171fa9a7c842439a8f2965aaab43c2f92
```

**5. Validate the task by running** 

```

[root@stapp01 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
cluster             devops              8eadaab68109        33 seconds ago      122MB
ubuntu              latest              c6b84b685f35        3 weeks ago         77.8MB
```

**6.  Click on `Finish` & `Confirm` to complete the task successfully.**