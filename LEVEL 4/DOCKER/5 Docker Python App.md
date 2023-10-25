


## Question

A python app needed to be Dockerized, and then it needs to be deployed on  `App Server 2`. We have already copied a  `requirements.txt`  file (having the app dependencies) under  `/python_app/src/`  directory on  `App Server 2`. Further complete this task as per details mentioned below:  


1.  Create a  `Dockerfile`  under  `/python_app`  directory:
    
    -   Use any  `python`  image as the base image.
    -   Install the dependencies using  `requirements.txt`  file.
    -   Expose the port  `3000`.
    -   Run the  `server.py`  script using  `CMD`.  
          
        
2.  Build an image named  `nautilus/python-app`  using this Dockerfile.  
      
    
3.  Once image is built, create a container named  `pythonapp_nautilus`:
    
    -   Map port  `3000`  of the container to the host port  `8091`.  
          
        
4.  Once deployed, you can test the app using  `curl`  command on  `App Server 2`.  
      
    

```sh
curl http://localhost:8091/
```

## Solution
**1. At first login on app server as per the task & Switch to root user**
```
thor@jump_host ~$ ssh steve@stapp02
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:GXiVgPiuZC04TfFFdi1eBmwDqrkiacT1Zij7KH95y+A.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02,172.16.238.11' (ECDSA) to the list of known hosts.
steve@stapp02's password: Am3ric@

[steve@stapp02 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: Am3ric@

```
**2. Create a  `Dockerfile`:**

```
[root@stapp02 ~]# cd /python_app
[root@stapp02 python_app]# ls
src
[root@stapp02 python_app]# vi Dockerfile
[root@stapp02 python_app]# cat Dockerfile
FROM python:3.9

COPY src .

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 3000

CMD ["python", "server.py"]
 
```
**3. Build image:**

```
[root@stapp02 python_app]# docker build -t nautilus/python-app .
Sending build context to Docker daemon  4.608kB
Step 1/5 : FROM python:3.9
3.9: Pulling from library/python
0a9573503463: Pull complete 
1ccc26d841b4: Pull complete 
800d84653581: Pull complete 
7c632e57ea62: Pull complete 
f9a1922eee8a: Pull complete 
6134ff8e5d41: Pull complete 
62aa274b0fd0: Pull complete 
fab8d0e2ef62: Pull complete 
Digest: sha256:87c2184bb76a2018d12aabdba026df504d0057bf3db29a301cfc4d4eb5f3c425
Status: Downloaded newer image for python:3.9
 ---> 33c1a19b1afb
Step 2/5 : COPY src .
 ---> 0dfeae997415
Step 3/5 : RUN pip install --no-cache-dir -r requirements.txt
 ---> Running in 57955bc4d6e3
Collecting flask
  Downloading flask-3.0.0-py3-none-any.whl (99 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 99.7/99.7 kB 5.0 MB/s eta 0:00:00
Collecting itsdangerous>=2.1.2
  Downloading itsdangerous-2.1.2-py3-none-any.whl (15 kB)
Collecting Jinja2>=3.1.2
  Downloading Jinja2-3.1.2-py3-none-any.whl (133 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 133.1/133.1 kB 253.9 MB/s eta 0:00:00
Collecting blinker>=1.6.2
  Downloading blinker-1.6.3-py3-none-any.whl (13 kB)
Collecting Werkzeug>=3.0.0
  Downloading werkzeug-3.0.0-py3-none-any.whl (226 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 226.6/226.6 kB 128.8 MB/s eta 0:00:00
Collecting importlib-metadata>=3.6.0
  Downloading importlib_metadata-6.8.0-py3-none-any.whl (22 kB)
Collecting click>=8.1.3
  Downloading click-8.1.7-py3-none-any.whl (97 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 97.9/97.9 kB 256.3 MB/s eta 0:00:00
Collecting zipp>=0.5
  Downloading zipp-3.17.0-py3-none-any.whl (7.4 kB)
Collecting MarkupSafe>=2.0
  Downloading MarkupSafe-2.1.3-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (25 kB)
Installing collected packages: zipp, MarkupSafe, itsdangerous, click, blinker, Werkzeug, Jinja2, importlib-metadata, flask
Successfully installed Jinja2-3.1.2 MarkupSafe-2.1.3 Werkzeug-3.0.0 blinker-1.6.3 click-8.1.7 flask-3.0.0 importlib-metadata-6.8.0 itsdangerous-2.1.2 zipp-3.17.0
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv

[notice] A new release of pip is available: 23.0.1 -> 23.3.1
[notice] To update, run: pip install --upgrade pip
Removing intermediate container 57955bc4d6e3
 ---> 484008248abf
Step 4/5 : EXPOSE 3000
 ---> Running in e042b57247a1
Removing intermediate container e042b57247a1
 ---> 1ca9248aff7a
Step 5/5 : CMD ["python", "server.py"]
 ---> Running in 65882104b2e7
Removing intermediate container 65882104b2e7
 ---> d4d64b89535b
Successfully built d4d64b89535b
Successfully tagged nautilus/python-app:latest

```
**4. Run the container image:**

```
[root@stapp02 python_app]# docker run -p 8091:3000 --name pythonapp_nautilus -d nautilus/python-app
a7c81a851070662948c60a26f0ce6be8d1b1c0e99472e839363159e6c0771b41


```
**5. Check the result:**

```
[root@stapp02 python_app]# curl http://localhost:8091
Welcome to xFusionCorp Industries!

```
**6. Click on  `Finish`  &  `Confirm`  to complete the task successful**

![pyapp](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/fc75fbca-5ee7-4c95-918f-d23e8fc6fd49)
