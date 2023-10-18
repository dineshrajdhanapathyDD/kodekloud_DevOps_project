## Questions

One of the DevOps engineers was working on to create a Dockerfile for Nginx. We need to build an image using that Dockerfile. The deployment must be done using a Jenkins pipeline. Below you can find more details about the same.  
  
Click on the  `Jenkins`  button on the top bar to access the Jenkins UI. Login using username  `admin`  and password  `Adm!n321`.  
 
Similarly, click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`. There is a repository named  `sarah/web`  in Gitea.  
  
Create/configure a Jenkins pipeline job named  `nginx-container`, configure it to run on server  `App Server 2`.  
  
-   The pipeline can have just one stage named  `Build`. (name is case sensitive)  
        
-   In the  `Build`  stage, build an image named  `stregi01.stratos.xfusioncorp.com:5000/nginx:latest`  using the  `Dockerfile`  present under the Git repository.  `stregi01.stratos.xfusioncorp.com:5000`  is the image registry server. After building the image push the same to the image registry server.  

`Note:`  
  
1.  You might need to install some plugins and restart Jenkins service. So, we recommend clicking on  `Restart Jenkins when installation is complete and no jobs are running`  on plugin installation/update page i.e  `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.  
        
2.  For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Solution:

1.  Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8081 and click on Display Port. You should be able to access the Jenkins login page. Login using username the  `admin`  and  `Adm!n321`  password.

![1 login page](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c8aca5f9-acf8-4acc-83ab-cbc07abd9110)

![2 dashboard](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9022c02a-0514-45eb-9221-bd72df59343e)

2.  Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.

![3 mange plugins](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ceea0387-e446-4c85-ad43-3243919c69d2)

Search for ssh , credentials, pipeline &git -> select all plugin checkbox click and click Download now and install after restart

![4 add plugin ](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3f0e20fb-1fc9-48bd-b4b0-70e44629b689)

![5 add plugins 1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/50b74085-3675-4b3d-83f7-9b97a6f06579)

- click Download now and install after restart.

![6 download progress](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/a9c99c41-feea-4f94-8fed-4fcba931657a)

-   Restart jenkins

3.  Click jenkins > manage jenkins > Credentials

![7 credentials](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/8d99ea42-02ef-409f-b5e8-b6b305cd7516)
 - Click credentials
 
 3.  Click jenkins > manage jenkins > Credentials > System > Global credentials(unrestricted)

Click add Credentials button

kind -> Username with password
Username -> steve
password -> Am3ric@
ID -> stapp02

![8 add credentials](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/daa7a98a-cd98-4975-bb50-69ee8213e6e7)
 - Click create.

![9 set global](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/a6e00133-543c-42c4-be2e-55aa8f078aa4)

4.  Add Manage node and cloud as per the task

Click jenkins > manage jenkins > Manage Nodes and clouds >

![10 manage nodes](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/b5544d1f-aa19-499f-adb6-a7359ce748e3)


![11 nodes](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/706b10bc-0281-43b8-b8f7-23fca0560e46)

- click New Node button

- Node name -> app02
- Type -> Permanent agent

![12 new node](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/7b9e5e07-e5e8-40f3-b4b8-548e4514e5df)


- Click create

```
Name -> app02

Number of executors -> 1

Remote root directory - /home/steve

labels : app02

launch method : launch agents via ssh

Credentials -> choose steve

Host key verification strategy -> non verify verfication strategy

Port -> 22
```

![13 node create](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d1a7fa35-3842-4eb1-93e4-c81fe7311962)

- Click save

- Create Node
![14 node finish](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/88b462d7-8362-4895-b2eb-7630aca89c9f)


- Check the launh agent for appo2

- Back to session terminal
```

thor@jump_host ~$ ssh steve@stapp02
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:9vWzV0gqJwkvtv/sNFfaE0A9Q9uqH9FozlUCPy22ZnY.
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
[root@stapp02 ~]# yum install -y java-17-openjdk.x86_64
```

![17 java version](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c9e265c4-934e-4806-9e98-08b62d02be26)


![18 java log ](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/1f5be225-f93d-4a57-8f8c-d837a674e2a1)
 - Agent successfully connected and online


- Create app02 node and cloud:
![19 create app02](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/fb594f9f-933c-430a-8a1b-75dc21f203b8)


5.  Create/configure a Jenkins pipeline job named  `nginx-container`, configure it to run on server  `App Server 2`.  

![20 new job](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/aedee368-6c68-43c0-8a82-9a2c9894a040)

![21 nginx build ready](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/46e3202c-c39d-48bb-b5ca-aea34e9b6945)


6. click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`. 

![15 gitea login sarah](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/8dd5160e-289c-4ce0-ae00-442bb70add80)


 -  Click the  repository named  `sarah/web`  in Gitea. copy the Git link

![16 sarah dashboard](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/eaa63529-f359-40b1-90ae-3a895bcff4b2)


- Back to jenkins >  Create the pipeline script.

```
pipeline {
    agent {
        label 'app02'
    }
    
    stages {
        stage('Build') {
            steps {
                git branch: "master",
                    url: "http://git.stratos.xfusioncorp.com/sarah/web.git"
                
                sh 'docker build -t stregi01.stratos.xfusioncorp.com:5000/nginx:latest .'
                sh 'docker push stregi01.stratos.xfusioncorp.com:5000/nginx:latest'
            }
        }
    }
}
```



![22 pipeline](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/046f5c2f-d013-4463-b23e-924b2ab85900)

- Back to Terminal Run Docker images
```
[root@stapp02 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
[root@stapp02 ~]#
```

- Build Now

![25 console output](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/f4eb10e9-775b-4d25-8fb9-c60e1366125d)

```
Console Output
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on app02 in /home/steve/workspace/nginx-container
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] git
The recommended git tool is: NONE
No credentials specified
Cloning the remote Git repository
Cloning repository http://git.stratos.xfusioncorp.com/sarah/web.git
 > git init /home/steve/workspace/nginx-container # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.3'
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
Checking out Revision dcdb76b2a6c7d59547c9c6d45658886ae002dc41 (refs/remotes/origin/master)
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f dcdb76b2a6c7d59547c9c6d45658886ae002dc41 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git checkout -b master dcdb76b2a6c7d59547c9c6d45658886ae002dc41 # timeout=10
Commit message: "Added dockerfile"
First time build. Skipping changelog.
[Pipeline] sh
+ docker build -t stregi01.stratos.xfusioncorp.com:5000/nginx:latest .
Sending build context to Docker daemon  62.46kB

Step 1/2 : FROM nginx:stable-alpine3.17-slim
stable-alpine3.17-slim: Pulling from library/nginx
9398808236ff: Pulling fs layer
708173787fc8: Pulling fs layer
b5b131b8c886: Pulling fs layer
ab69664ce136: Pulling fs layer
d7f3c29ebbc5: Pulling fs layer
80b006910f42: Pulling fs layer
ab69664ce136: Waiting
d7f3c29ebbc5: Waiting
80b006910f42: Waiting
b5b131b8c886: Verifying Checksum
b5b131b8c886: Download complete
708173787fc8: Verifying Checksum
708173787fc8: Download complete
9398808236ff: Verifying Checksum
9398808236ff: Download complete
ab69664ce136: Verifying Checksum
ab69664ce136: Download complete
80b006910f42: Download complete
d7f3c29ebbc5: Verifying Checksum
d7f3c29ebbc5: Download complete
9398808236ff: Pull complete
708173787fc8: Pull complete
b5b131b8c886: Pull complete
ab69664ce136: Pull complete
d7f3c29ebbc5: Pull complete
80b006910f42: Pull complete
Digest: sha256:b8132df8c2fc73f4c1e7ce434c1ff19b134818e8173cd5e8f79c55a5f635d7e5
Status: Downloaded newer image for nginx:stable-alpine3.17-slim
 ---> 517cf77e51a3
Step 2/2 : COPY index.html /usr/share/nginx/html/
 ---> 057527bc71a5
Successfully built 057527bc71a5
Successfully tagged stregi01.stratos.xfusioncorp.com:5000/nginx:latest
[Pipeline] sh
+ docker push stregi01.stratos.xfusioncorp.com:5000/nginx:latest
The push refers to repository [stregi01.stratos.xfusioncorp.com:5000/nginx]
cefdffd84016: Preparing
2b60bbe779e0: Preparing
4c6a1307a10b: Preparing
bb0903fd6f90: Preparing
9c01e5b3bd66: Preparing
57b608dd7b54: Preparing
36b50b131297: Preparing
57b608dd7b54: Waiting
36b50b131297: Waiting
bb0903fd6f90: Pushed
2b60bbe779e0: Pushed
9c01e5b3bd66: Pushed
4c6a1307a10b: Pushed
cefdffd84016: Pushed
36b50b131297: Pushed
57b608dd7b54: Pushed
latest: digest: sha256:68f9e724eb65d5f91a8bd02c8f9753e0a57f4ed9ee7ed0f9f47ced60dbbab674 size: 1775
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```
- After Build. 

![23 docker images](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e93090af-8b08-4e27-bd2c-43395f31338e)


```
[root@stapp02 ~]# docker images
REPOSITORY                                    TAG                      IMAGE ID            CREATED             SIZE
stregi01.stratos.xfusioncorp.com:5000/nginx   latest                   057527bc71a5        33 seconds ago      11.5MB
nginx                                         stable-alpine3.17-slim   517cf77e51a3        2 months ago        11.5MB
[root@stapp02 ~]# 
```

- Build Success

![26 build succes](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/729b3f4d-c370-413f-93c3-b955348b8931)


7.  Click on Finish & Confirm to complete the task successful




