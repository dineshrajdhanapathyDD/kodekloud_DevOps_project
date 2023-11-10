

## Questions:

The Nautilus application development team shared static website content that needs to be hosted on the `httpd` web server using a containerised platform. The team has shared details with the DevOps team, and we need to set up an environment according to those guidelines. Below are the details:

a. On `App Server 3` in `Stratos DC` create a container named `httpd` using a docker compose file `/opt/docker/docker-compose.yml` (please use the exact name for file).

b. Use `httpd` (preferably `latest` tag) image for container and make sure container is named as `httpd`; you can use any name for service.

c. Map `80` number port of container with port `3002` of docker host.


d. Map container's `/usr/local/apache2/htdocs` volume with `/opt/finance` volume of docker host which is already there. (please do not modify any data within these locations).



## Solution:

**1. Login to the app server 1 and switch to root.**

```

[root@stapp01 ~]# ssh banner@stapp03
banner@stapp03's password: BigGr33n

[banner@stapp03 ~]$ sudo su -

We trust you have received the usual lecture from the local System Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: BigGr33n
[root@stapp03 ~]# 
```

**2. Proceed to the specified directory and create the docker-compose.yml based on the requirements.**

```

[root@stapp03 ~]# cd /opt/docker

[root@stapp03 docker]# vi docker-compose.yml
[root@stapp03 docker]# cat docker-compose.yml
version: '3'
services:
  httpd-container:
    image: httpd:latest
    container_name: httpd
    ports:
      - "3002:80"
    volumes:
      - /opt/finance:/usr/local/apache2/htdocs
[root@stapp03 docker]# 
```

**3. Start the container and verify.**

```

[root@stapp03 docker]# docker-compose up -d
Creating network "docker_default" with the default driver
Pulling httpd-container (httpd:latest)...
latest: Pulling from library/httpd
a803e7c4b030: Pull complete
053327351b4a: Pull complete
de42e9dfbbe1: Pull complete
9d28e265584b: Pull complete
ce95f18e49ae: Pull complete
Digest: sha256:5123fb6e039b83a4319b668b4fe1ee04c4fbd7c4c8d1d6ef843e8a943a9aed3f
Status: Downloaded newer image for httpd:latest
Creating httpd ... done
[root@stapp03 docker]# 


[root@stapp03 docker]# docker ps
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS                  NAMES
c9f4b5fd09d3        httpd:latest        "httpd-foreground"   38 seconds ago      Up 31 seconds       0.0.0.0:3002->80/tcp   httpd
[root@stapp03 docker]# 
```

**4. We can try to curl the localhost via port 3002.**

```

[root@stapp03 docker]# curl -I http://localhost:3002
HTTP/1.1 200 OK
Date: Sat, 30 Sep 2023 17:58:30 GMT
Server: Apache/2.4.57 (Unix)
Content-Type: text/html;charset=ISO-8859-1

[root@stapp03 docker]# curl http://localhost:3002
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /</title>
 </head>
 <body>
<h1>Index of /</h1>
<ul><li><a href="index1.html"> index1.html</a></li>
</ul>
</body></html>
[root@stapp03 docker]# 
```

**5. Click on `Finish` & `Confirm` to complete the task successful**

