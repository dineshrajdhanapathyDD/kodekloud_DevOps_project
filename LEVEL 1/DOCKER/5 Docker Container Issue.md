

## Questions:

There is a static website running within a container named `nautilus`, this container is running on `App Server 1`. Suddenly, we started facing some issues with the static website on `App Server 1`. Look into the issue to fix the same, you can find more details below:

Container's volume `/usr/local/apache2/htdocs` is mapped with the host's volume `/var/www/html`.

The website should run on host port `8080` on `App Server 1` i.e command `curl http://localhost:8080/` should work on `App Server 1`.


## Solution:  

**1. At first login on app server  &  Switch to the root user**

```
thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n
```

**2. Run the Below command to check existing docker images**

```
[root@stapp01 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
httpd               latest              96a2d0570deb        13 days ago         168MB
```

**3. Run the below command and see all container**

```
[tony@stapp01 ~]$ docker ps -a
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS                     PORTS               NAMES
38053b0253d8        httpd               "httpd-foreground"   2 minutes ago       Exited (0) 2 minutes ago                       nautilus
[tony@stapp01 ~]$ docker run -d -v /var/www/html:/usr/local/apache2/htdocs -p 80:8080 --rm httpd:latest
0152b8e44ea41aa9afc3d7d25c6e55b17fe20cd8eb860997b6ac92888e79af57
```

**4. use the container id or the name nautilus” to restart the webserver**

```
[tony@stapp01 ~]$ docker restart nautilus
nautilus
```

**5. run the container with port**

```
[tony@stapp01 ~]$ docker run -d -v /var/www/html:/usr/local/apache2/htdocs -p 80:8080 --rm httpd:latest
7bdf31ded68ffa0058c36a7b6404319fff31999d7a9e52b190fcc5e35d5e1e26
```

**6. run the curl command:**

```
[tony@stapp01 ~]$ curl http://localhost:8080/
Welcome to xFusionCorp Industries!
```

**7.  Click on `Finish` & `Confirm` to complete the task successful**













