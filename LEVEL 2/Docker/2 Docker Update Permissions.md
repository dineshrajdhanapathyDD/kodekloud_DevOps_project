
## Questions:

One of the Nautilus project developers need access to run docker commands on `App Server 1`. This user is already created on the server. Accomplish this task as per details given below:


- User `javed` is not able to run docker commands on `App Server 1` in Stratos DC, make the required changes so that this user can run docker commands without `sudo`.

## Solution

**1.Run docker command**

```

[root@stapp02 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

**2. Now you can start Docker  with the following command with issue happen**

```

[root@stapp02 ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2023-09-11 12:58:23 UTC; 2min 13s ago
     Docs: https://docs.docker.com
 Main PID: 930 (dockerd)
    Tasks: 28
   Memory: 49.0M
   CGroup: /docker/422121962d2b9b47a52c3a4fa04307f6eef4df4cbda6ec844ddab307a77f5bfe/system.slice/docker.service
           └─930 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Sep 11 12:58:22 stapp02.stratos.xfusioncorp.com dockerd[930]: time="2023-09-11T12:58:22.685938195Z" level=info msg="Default bridge (docker0) is assigned with an IP address 172.18.0.0/16. Da>
Sep 11 12:58:22 stapp02.stratos.xfusioncorp.com dockerd[930]: time="2023-09-11T12:58:22.913326599Z" level=info msg="Loading containers: done."
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com dockerd[930]: time="2023-09-11T12:58:23.088257699Z" level=info msg="Docker daemon" commit=99e3ed8919 graphdriver(s)=vfs version=19.03.15
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com dockerd[930]: time="2023-09-11T12:58:23.088350627Z" level=info msg="Daemon has completed initialization"
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com dockerd[930]: time="2023-09-11T12:58:23.233905647Z" level=info msg="API listen on /var/run/docker.sock"
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Got notification message from PID 930 (READY=1)
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Changed start -> running
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Job docker.service/start finished, result=done
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com systemd[1]: Started Docker Application Container Engine.
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Failed to send unit change signal for docker.service: Connection reset by peer

[root@stapp02 ~]# systemctl start docker
```

**3. Change the permissions of docker socket to be able to connect to the docker daemon `/var/run/docker.sock`**

```
[root@stapp02 ~]# sudo chmod 666 /var/run/docker.sock  
```

**4. Make sure docker socket file exist with correct parameters** 

```

[root@stapp02 ~]# rpm -ql docker-ce-20.10.13-3.el7.x86_64

/usr/bin/docker-init
/usr/bin/docker-proxy
/usr/bin/dockerd
/usr/lib/systemd/system/docker.service
/usr/lib/systemd/system/docker.socket


[root@stapp02 ~]# vi /usr/lib/systemd/system/docker.socket
[root@stapp02 ~]# cat /usr/lib/systemd/system/docker.socket
[Unit]
Description=Docker Socket for the API
PartOf=docker.service

[Socket]
ListenStream=/var/run/docker.sock
SocketMode=0660
SocketUser=root
SocketGroup=docker

[Install]
WantedBy=sockets.target
```

**5. Add your user to the Docker**

```

[root@stapp02 ~]# ls -l /var/run/docker.sock
srw-rw---- 1 root docker 0 Sep 11 12:58 /var/run/docker.sock

root@stapp02 ~]# usermod -a -G docker javed
```

**6. If you face above  error while start docker, then run below command to resolve issue** 

```

[root@stapp02 ~]# systemctl start docker
[root@stapp02 ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2023-09-11 12:58:23 UTC; 13min ago
     Docs: https://docs.docker.com
 Main PID: 930 (dockerd)
    Tasks: 29
   Memory: 49.0M
   CGroup: /docker/422121962d2b9b47a52c3a4fa04307f6eef4df4cbda6ec844ddab307a77f5bfe/system.slice/docker.service
           └─930 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com systemd[1]: Started Docker Application Container Engine.
Sep 11 12:58:23 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Failed to send unit change signal for docker.service: Connection reset by peer
Sep 11 13:01:43 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Trying to enqueue job docker.service/start/replace
Sep 11 13:01:43 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Installed new job docker.service/start as 96
Sep 11 13:01:43 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Enqueued job docker.service/start as 96
Sep 11 13:01:43 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Job docker.service/start finished, result=done
Sep 11 13:11:27 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Trying to enqueue job docker.service/start/replace
Sep 11 13:11:27 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Installed new job docker.service/start as 130
Sep 11 13:11:27 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Enqueued job docker.service/start as 130
Sep 11 13:11:27 stapp02.stratos.xfusioncorp.com systemd[1]: docker.service: Job docker.service/start finished, result=done
```

**7. Validate by running**

```

[root@stapp02 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

**8. Click on `Finish` & `Confirm` to complete the task successful**