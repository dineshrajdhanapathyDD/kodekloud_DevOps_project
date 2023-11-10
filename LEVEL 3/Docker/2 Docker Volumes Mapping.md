

## Questions: 

The Nautilus DevOps team is testing applications containerization, which issupposed to be migrated on docker container-based environments soon. In today's stand-up meeting one of the team members has been assigned a task to create and test a docker container with certain requirements. Below are more details:


a. On `App Server 1` in `Stratos DC` pull `nginx` image preferably `latest` tag but others should work too.


b. Create a new container with name `ecommerce` from the image you just pulled.


c. Map the host volume `/opt/itadmin` with container volume `/tmp`. There is an `sample.txt` file present on same server under `/tmp`; copy that file to `/opt/itadmin`. Also please keep the container in running state.



 ## Solution:  


**1. At first login on app server  &  Switch to the root user** 

```

thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:jHEPTGeWH5uQqzrJDXZWyC494oIiKga+uQf0/5Nw+xs.
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

[root@stapp01 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

**3. Run the Below command to  pull the docker image on the server**

```

[root@stapp01 ~]# docker pull nginx:latest
latest: Pulling from library/nginx
a803e7c4b030: Pull complete 
8b625c47d697: Pull complete 
4d3239651a63: Pull complete 
0f816efa513d: Pull complete 
01d159b8db2f: Pull complete 
5fb9a81470f3: Pull complete 
9b1e1e7164db: Pull complete 
Digest: sha256:32da30332506740a2f7c34d5dc70467b7f14ec67d912703568daff790ab3f755
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

**4. copy file `/tmp/sample.txt`  to `/opt dba`**

```

[root@stapp01 ~]# cp /tmp/sample.txt /opt/itadmin
```

**5. Run docker container from the image with given name and path** 

```

[root@stapp01 ~]# docker run --name ecommerce -v /opt/itadmin:/tmp -d -it nginx:latest
9be5507a04bf2465edb9622ede928e1c3bb03e3a590183bc13192f7d25206406
```

**6. Validate the task by login on  docker container and list  `/tmp`**

```

[root@stapp01 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
9be5507a04bf        nginx:latest        "/docker-entrypoint.…"   34 seconds ago      Up 32 seconds       80/tcp              ecommerce 

[root@stapp01 ~]# docker exec -it 9be5507a04bf /bin/bash

root@9be5507a04bf:/# ls /tmp
sample.txt
```

**7.  Click on `Finish` & `Confirm` to complete the task successful**
