

## Questions:

One of the DevOps team members was working on to create a new custom docker image on `App Server 1` in `Stratos DC`. He is done with his changes and image is saved on same server with name `official:devops`. Recently a requirement has been raised by a team to use that image for testing, but the team wants to test the same on `App Server 3`. So we need to provide them that image on `App Server 3` in `Stratos DC`.


a. On `App Server 1` save the image `official:devops` in an archive.


b. Transfer the image archive to `App Server 3`.


c. Load that image archive on `App Server 3` with same name and tag which was used on `App Server 1`.


`Note`: Docker is already installed on both servers; however, if its service is down please make sure to start it.


## Solution:


**1. Login on  App server as per the task**

```

thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:vpC8+T5cEj8K3QbGJbC5fRaWjkc7NpM2xhW366uwEiI.
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

**2. List the existing images in docker which need to archive**

```

[root@stapp01 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
official            devops              740ab14c41e7        4 minutes ago       122MB
ubuntu              latest              c6b84b685f35        6 weeks ago         77.8MB
```

**3. save the image official:devops in an archive**

```

[root@stapp01 ~]# docker save -o /tmp/official.tar official:devops
[root@stapp01 ~]# 

root@stapp01 ~]# ls /tmp/
docker_container.sh  ks-script-kzk1nzfd  ks-script-l36mq90h  official.tar
```

**4. Copy the tar file on Stapp03 app server**

```

[root@stapp01 ~]# scp /tmp/official.tar banner@stapp03:/tmp
The authenticity of host 'stapp03 (172.16.238.12)' can't be established.
ECDSA key fingerprint is SHA256:UuKXr1P6UiFrDwtb5HjfhTAsDtpm4qFOZnYd88w9sU0.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.
banner@stapp03's password: BigGr33n
official.tar                                                                              100%  119MB  99.9MB/s   00:01 
```

**5. Login on  Stapp03 app server & switch to root user**

```

[root@stapp01 ~]# ssh banner@stapp03
banner@stapp03's password: BigGr33n
[banner@stapp03 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: BigGr33n
[root@stapp03 ~]# 
```

**6. Go to tmp folder and confirm the tar file copied successfully**

```

[root@stapp03 tmp]# ls /tmp/
ks-script-kzk1nzfd  ks-script-l36mq90h  official.tar
```

**7. Start Docker service if its not active**

```

[root@stapp03 tmp]# systemctl start docker
[root@stapp03 tmp]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2023-09-29 12:10:13 UTC; 16min ago
     Docs: https://docs.docker.com
 Main PID: 595 (dockerd)
    Tasks: 33
   Memory: 51.6M
   CGroup: /docker/413d76df9711de1ea2e45993cd4825703afe4e0491861e8f619793a22be7425d/system.slice/docker.service
           └─595 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Sep 29 12:10:13 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Job docker.service/start finished, result=done
Sep 29 12:10:13 stapp03.stratos.xfusioncorp.com systemd[1]: Started Docker Application Container Engine.
Sep 29 12:22:24 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Trying to enqueue job docker.service/start/repl>
Sep 29 12:22:24 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Installed new job docker.service/start as 61
Sep 29 12:22:24 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Enqueued job docker.service/start as 61
Sep 29 12:22:24 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Job docker.service/start finished, result=done
Sep 29 12:26:10 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Trying to enqueue job docker.service/start/repl>
Sep 29 12:26:10 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Installed new job docker.service/start as 99
Sep 29 12:26:10 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Enqueued job docker.service/start as 99
Sep 29 12:26:10 stapp03.stratos.xfusioncorp.com systemd[1]: docker.service: Job docker.service/start finished, result=done
```

**8. Load that image archive**

```

[root@stapp03 tmp]# docker load -i official.tar
dc0585a4b8b7: Loading layer [==================================================>]  80.35MB/80.35MB
bb26dd7e408a: Loading layer [==================================================>]  44.17MB/44.17MB
Loaded image: official:devops
```

**9. Validate the task by docker images**

```

[root@stapp03 tmp]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
official            devops              740ab14c41e7        17 minutes ago      122MB
```

**10. Click on `Finish` & `Confirm` to complete the task successful**
