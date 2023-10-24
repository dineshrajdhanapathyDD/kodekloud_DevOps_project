


## Question


There is a requirement to Dockerize a Node app and to deploy the same on  `App Server 1`. Under  `/node_app`  directory on  `App Server 1`, we have already placed a  `package.json`  file that describes the app dependencies and  `server.js`  file that defines a web app framework.  
  

  

1.  Create a  `Dockerfile`  (name is case sensitive) under  `/node_app`  directory:
    
    -   Use any  `node`  image as the base image.
    -   Install the dependencies using  `package.json`  file.
    -   Use  `server.js`  in the  `CMD`.
    -   Expose port  `3001`.  
          
        
2.  The build image should be named as  `nautilus/node-web-app`.  
      
    
3.  Now run a container named  `nodeapp_nautilus`  using this image.
    
    -   Map the container port  `3001`  with the host port  `8096`.  
          
        

. Once deployed, you can test the app using a  `curl`  command on  `App Server 1`:  
  
  

`curl http://localhost:8096`


## Solution
**1. At first login on app server as per the task & Switch to root user**
```
thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:h9kurQM0sEbOZ9HaepxEv85YOGOa7NytBowIWox7GRQ.
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

**2. Create a  `Dockerfile`:**

```
[root@stapp01 ~]# cd /node_app
[root@stapp01 node_app]# ls
package.json  server.js
[root@stapp01 node_app]# vi Dockerfile
[root@stapp01 node_app]# cat Dockerfile
FROM node:18-buster-slim

COPY . .

RUN npm install

EXPOSE 3001

CMD ["node", "server.js"]
[root@stapp01 node_app]# 

```

**3. Build image:**
```
[root@stapp01 node_app]# docker build -t nautilus/node-web-app .
Sending build context to Docker daemon  4.096kB
Step 1/5 : FROM node:18-buster-slim
18-buster-slim: Pulling from library/node
b70638ed4228: Pull complete 
7ec5d71a64dc: Pull complete 
0df53c35e18f: Pull complete 
70b562a4bd14: Pull complete 
573a59ab8763: Pull complete 
Digest: sha256:0f03f9a3d306cec7fb33910ce494e0aeffec2e00ca4e4d92851c8787aca4e142
Status: Downloaded newer image for node:18-buster-slim
 ---> 4dfd70839a4c
Step 2/5 : COPY . .
 ---> 774024d723c5
Step 3/5 : RUN npm install
 ---> Running in 9af03b89dc89

added 62 packages, and audited 63 packages in 4s

11 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice 
npm notice New major version of npm available! 9.8.1 -> 10.2.1
npm notice Changelog: <https://github.com/npm/cli/releases/tag/v10.2.1>
npm notice Run `npm install -g npm@10.2.1` to update!
npm notice 
Removing intermediate container 9af03b89dc89
 ---> 28ddf34416f2
Step 4/5 : EXPOSE 3001
 ---> Running in 5a01ae1c2b61
Removing intermediate container 5a01ae1c2b61
 ---> 5b96f465f43f
Step 5/5 : CMD ["node", "server.js"]
 ---> Running in 28bab04942fd
Removing intermediate container 28bab04942fd
 ---> 53c2f88abd12
Successfully built 53c2f88abd12
Successfully tagged nautilus/node-web-app:latest
[root@stapp01 node_app]# 
```

**4. Run the container image:**

```
[root@stapp01 node_app]# docker run -p 8096:3001 --name nodeapp_nautilus -d nautilus/node-web-app
01f6a02e9205022f26f1e18b2869fc65c841c2fbce835d5f6a35b1b2d33e006a
```
**5. Running status**
```
[root@stapp01 node_app]# docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                    NAMES
01f6a02e9205        nautilus/node-web-app   "docker-entrypoint.s…"   34 seconds ago      Up 28 seconds       0.0.0.0:8096->3001/tcp   nodeapp_nautilus
[root@stapp01 node_app]# 
```

**6. Check the result:**
```
[root@stapp01 node_app]# curl http://localhost:8096
Welcome to xFusionCorp Industries![root@stapp01 node_app]# 
```
**7. Click on  `Finish`  &  `Confirm`  to complete the task successful**

![ser](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/1577b258-fb59-4023-93bf-b7f83e690609)
