


## Question




The Nautilus DevOps team is working to deploy one of the applications on  `App Server 2`  in  `Stratos DC`. Due to a misconfiguration in the docker compose file, the deployment is failing. We would like you to take a look into it to identify and fix the issues. More details can be found below:

  

a.  `docker-compose.yml`  file is present on  `App Server 2`  under  `/opt/docker`  directory.  
  

b. Try to run the same and make sure it works fine.  
  

c. Please do not change the  `container names`  being used. Also, do not update or alter any other valid config settings in the compose file or any other relevant data that can cause app failure.  
  

`Note:`  Please note that once you click on  `FINISH`  button all existing running/stopped containers will be destroyed, and your compose will be run.


## Solution 

**1. At first login on app server as per the task & Switch to root user**
```
thor@jump_host ~$ ssh steve@stapp02
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:rbPC/QfoMRBshfRpyMQtN7x1gqzv62G2pFFH6lhs3To.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02,172.16.238.11' (ECDSA) to the list of known hosts.
steve@stapp02's password: 
[steve@stapp02 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[root@stapp02 ~]# 

```
**2. check the existing file `docker-compose.yml` under `/opt/docker` directory. File has issue need to rectify**

```
[root@stapp02 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@stapp02 ~]# cd /opt/docker/
[root@stapp02 docker]# ls
app  docker-compose.yml
[root@stapp02 docker]# cat docker-compose.yml
version: '2'
services:
    web:
        image: ./app
        container_name: python
        port:
            - "5000:5000"
        volumes:
            - ./app:/code
        depends_on:
            - redis
    redis_app:
        image: redis
        container_name: redis[root@stapp02 docker]# 
```

**3. Edit the compose file and rectify the changes**

```
[root@stapp02 docker]# vi docker-compose.yml
[root@stapp02 docker]# cat docker-compose.yml
version: '2'
services:
    web:
        build: ./app
        container_name: python
        ports:
            - "5000:5000"
        volumes:
            - ./app:/code
        depends_on:
            - redis_app
    redis_app:
        image: redis
        container_name: redis
[root@stapp02 docker]# 

```

  
**4. Post file changes save and run to build**

```

[root@stapp02 docker]# docker-compose up
Creating network "docker_default" with the default driver
Pulling redis_app (redis:)...
latest: Pulling from library/redis
a378f10b3218: Pull complete
b266cd8112a6: Pull complete
7ba86e6448de: Pull complete
3aeb7c9e9a5f: Pull complete
de3be2a98bda: Pull complete
4f4fb700ef54: Pull complete
98e18d21aa3b: Pull complete
Building web
Step 1/5 : FROM python:2.7
2.7: Pulling from library/python
7e2b2a5af8f6: Pull complete
09b6f03ffac4: Pull complete
dc3f0c679f0f: Pull complete
fd4b47407fc3: Pull complete
b32f6bf7d96d: Pull complete
6f4489a7e4cf: Pull complete
af4b99ad9ef0: Pull complete
39db0bc48c26: Pull complete
acb4a89489fc: Pull complete
Digest: sha256:cfa62318c459b1fde9e0841c619906d15ada5910d625176e24bf692cf8a2601d
Status: Downloaded newer image for python:2.7
 ---> 68e7be49c28c
Step 2/5 : ADD . /code
 ---> 73bda7e43064
Step 3/5 : WORKDIR /code
 ---> Running in 9f5e287a7832
Removing intermediate container 9f5e287a7832
 ---> f092a4fe28b6
Step 4/5 : RUN pip install -r requirements.txt
 ---> Running in 7906ac06b759
DEPRECATION: Python 2.7 reached the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 is no longer maintained. A future version of pip will drop support for Python 2.7. More details about Python 2 support in pip, can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support
Collecting flask
  Downloading Flask-1.1.4-py2.py3-none-any.whl (94 kB)
Collecting redis
  Downloading redis-3.5.3-py2.py3-none-any.whl (72 kB)
Collecting click<8.0,>=5.1
  Downloading click-7.1.2-py2.py3-none-any.whl (82 kB)
Collecting Werkzeug<2.0,>=0.15
  Downloading Werkzeug-1.0.1-py2.py3-none-any.whl (298 kB)
Collecting Jinja2<3.0,>=2.10.1
  Downloading Jinja2-2.11.3-py2.py3-none-any.whl (125 kB)
Collecting itsdangerous<2.0,>=0.24
  Downloading itsdangerous-1.1.0-py2.py3-none-any.whl (16 kB)
Collecting MarkupSafe>=0.23
  Downloading MarkupSafe-1.1.1-cp27-cp27mu-manylinux1_x86_64.whl (24 kB)
Installing collected packages: click, Werkzeug, MarkupSafe, Jinja2, itsdangerous, flask, redis
Successfully installed Jinja2-2.11.3 MarkupSafe-1.1.1 Werkzeug-1.0.1 click-7.1.2 flask-1.1.4 itsdangerous-1.1.0 redis-3.5.3
WARNING: You are using pip version 20.0.2; however, version 20.3.4 is available.
You should consider upgrading via the '/usr/local/bin/python -m pip install --upgrade pip' command.
Removing intermediate container 7906ac06b759
 ---> d5aa9f445289
Step 5/5 : CMD python app.py
 ---> Running in 0f611e2f3022
Removing intermediate container 0f611e2f3022
 ---> 8c2d363c8560
Successfully built 8c2d363c8560
Successfully tagged docker_web:latest
WARNING: Image for service web was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating redis ... done
Creating python ... done
Attaching to redis, python
redis        | 1:C 22 Oct 2023 08:35:43.449 # WARNING Memory overcommit must be enabled! Without it, a background save or replication may fail under low memory condition. Being disabled, it can also cause failures without low memory condition, see https://github.com/jemalloc/jemalloc/issues/1328. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
redis        | 1:C 22 Oct 2023 08:35:43.449 * oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis        | 1:C 22 Oct 2023 08:35:43.449 * Redis version=7.2.2, bits=64, commit=00000000, modified=0, pid=1, just started
redis        | 1:C 22 Oct 2023 08:35:43.449 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis        | 1:M 22 Oct 2023 08:35:43.450 * monotonic clock: POSIX clock_gettime
redis        | 1:M 22 Oct 2023 08:35:43.450 * Running mode=standalone, port=6379.
redis        | 1:M 22 Oct 2023 08:35:43.452 * Server initialized
redis        | 1:M 22 Oct 2023 08:35:43.452 * Ready to accept connections tcp
python       |  * Serving Flask app "app" (lazy loading)
python       |  * Environment: production
python       |    WARNING: This is a development server. Do not use it in a production deployment.
python       |    Use a production WSGI server instead.
python       |  * Debug mode: on
python       |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
python       |  * Restarting with stat
python       |  * Debugger is active!
python       |  * Debugger PIN: 264-849-677
python       | 172.18.0.1 - - [22/Oct/2023 08:39:00] "GET / HTTP/1.1" 200 -
```

**5. Open new terminal check existing docker container running**

```
thor@jump_host ~$ ssh steve@stapp02
steve@stapp02's password: 
Last login: Sun Oct 22 08:27:37 2023 from jump_host.devops-docker-compose-issues-v2_app_net
[steve@stapp02 ~]$ 
[steve@stapp02 ~]$ sudo su -
[sudo] password for steve: 
Last login: Sun Oct 22 08:27:45 UTC 2023 on pts/0
[root@stapp02 ~]# 
[root@stapp02 ~]# docker ps
CONTAINER ID   IMAGE        COMMAND                  CREATED         STATUS              PORTS                    NAMES
5dd0f25ab327   docker_web   "/bin/sh -c 'python …"   2 minutes ago   Up About a minute   0.0.0.0:5000->5000/tcp   python
2a2fc315a23a   redis        "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes        6379/tcp                 redis
```

**6. Validate the task by curl the port**

```
[root@stapp02 ~]# curl -ik http://localhost:5000
HTTP/1.0 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 50
Server: Werkzeug/1.0.1 Python/2.7.18
Date: Sun, 22 Oct 2023 08:39:00 GMT

This Compose/Flask demo has been viewed 1 time(s).[root@stapp02 ~]# 
```

7. **Click on `Finish` & `Confirm` to complete the task successful**

![5](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0b89f8ac-d315-4211-9d67-2d6c49b79306)
