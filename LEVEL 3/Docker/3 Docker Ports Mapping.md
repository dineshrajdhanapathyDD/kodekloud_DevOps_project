

## Questions:

The Nautilus DevOps team is planning to host an application on a nginx-based container. There are number of tickets already been created for similar tasks. One of the tickets has been assigned to set up a nginx container on `Application Server 3` in `Stratos Datacenter`. Please perform the task as per details mentioned below:


a. Pull `nginx:alpine-perl` docker image on `Application Server 3`.


b. Create a container named `news` using the image you pulled.


c. Map host port `3002` to container port `80`. Please keep the container in running state.


## Solution:  


**1. At first login on app server  ssh banner@stapp03**

```

thor@jump_host ~$ ssh banner@stapp03
The authenticity of host 'stapp03 (172.16.238.12)' can't be established.
ECDSA key fingerprint is SHA256:aiKm4mbfc9oUI9AUjvyk3cM+60x7IRy+fI4EZWLYy4M.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.
banner@stapp03's password: BigGr33n
[banner@stapp03 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: BigGr33n
```

**2. Run Below command to check existing docker images** 

```

[root@stapp03 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

**3.  Pull Docker image  on server** 

```

[root@stapp03 ~]# docker pull nginx:alpine-perl
alpine-perl: Pulling from library/nginx
7264a8db6415: Pull complete 
518c62654cf0: Pull complete 
d8c801465ddf: Pull complete 
ac28ec6b1e86: Pull complete 
eb8fb38efa48: Pull complete 
e92e38a9a0eb: Pull complete 
58663ac43ae7: Pull complete 
2f545e207252: Pull complete 
09a186681e2f: Pull complete 
Digest: sha256:7c2df7ec59565a04aee121a358dd8ac25ca5ce111ada9e11723b27ca114fc6fb
Status: Downloaded newer image for nginx:alpine-perl
docker.io/library/nginx:alpine-perl
```

**4. Verify the pull image**

```

[root@stapp03 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               alpine-perl         da842610a485        4 weeks ago         78MB
```

**5. Run below command to run container**

```

[root@stapp03 ~]# docker container run -d --name news -p 3002:80 nginx:alpine-perl
38c4574aaca8dcd85949a0715aa3d50cc62ac9ea955f181c6a15f08896e33b20
```

**6. Validate the task by below command and curl** 

```

[root@stapp03 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
38c4574aaca8        nginx:alpine-perl   "/docker-entrypoint.…"   28 seconds ago      Up 25 seconds       0.0.0.0:3002->80/tcp   news
[root@stapp03 ~]# curl http://localhost:3002
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

**7.  Click on `Finish` & `Confirm` to complete the task successful**
