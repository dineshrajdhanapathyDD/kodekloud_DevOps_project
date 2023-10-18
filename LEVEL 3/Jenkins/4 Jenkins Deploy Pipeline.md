

## Questions

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:  
 
Click on the  `Jenkins`  button on the top bar to access the Jenkins UI. Login using username  `admin`  and password  `Adm!n321`.  
  
Similarly, click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`. There under user  `sarah`  you will find a repository named  `web_app`  that is already cloned on  `Storage server`  under  `/var/www/html`. sarah is a developer who is working on this repository.  
  
1.  Add a slave node named  `Storage Server`. It should be labeled as  `ststor01`  and its remote root directory should be  `/var/www/html`.  
      
2.  We have already cloned repository on  `Storage Server`  under  `/var/www/html`.  
        
3.  Apache is already installed on all app Servers its running on port  `8080`.  
   
4.  Create a Jenkins pipeline job named  `devops-webapp-job`  (it must not be a  `Multibranch pipeline`) and configure it to:  
      
   -  Deploy the code from  `web_app`  repository under  `/var/www/html`  on  `Storage Server`, as this location is already mounted to the document root  `/var/www/html`  of app servers. The pipeline should have a single stage named  `Deploy`  ( which is case sensitive ) to accomplish the deployment.  
          
        

LB server is already configured. You should be able to see the latest changes you made by clicking on the  `App`  button. Please make sure the required content is loading on the main URL  `https://<LBR-URL>`  i.e there should not be a sub-directory like  `https://<LBR-URL>/web_app`  etc.  
  

`Note:`  
  

1.  You might need to install some plugins and restart Jenkins service. So, we recommend clicking on  `Restart Jenkins when installation is complete and no jobs are running`  on plugin installation/update page i.e  `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.  
      
    
2.  For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Solution:


1.  **Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8081 and click on Display Port. You should be able to access the Jenkins login page. Login using username the  `admin`  and  `Adm!n321`  password.**

![1 login page.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/20034079-e0c5-41a9-b082-135934654900)

![2 dash.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0ba07b1d-b81b-4a4c-b06c-41831affde0e)

2.  **Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.**

![3 manage plugins.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/6afe70d5-c791-4d85-b24e-924438845d5c)


-  Search for ssh , credentials, pipeline & git -> select all plugin checkbox click and click Download now and install after restart.

![4 add plugin.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/fd022ba8-a5f9-41ee-8dba-7d73fd05797b)

-   click Download now and install after restart.

![5 1 download poess.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4d5b3766-a902-4216-8a3e-25aca0c047cb)

![5 2 restart.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/8c5f3ae4-00f3-4189-bd4b-318da76dd409)

-   Restart jenkins

3. **Click jenkins > manage jenkins > Credentials**

![7 credential.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/2f0f3356-3777-4942-8c18-b79487c6e734)

-   Click credentials

Click jenkins > manage jenkins > Credentials > System 

![8.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/58f1f88a-9fed-4e74-9a51-aa61dabf2646)

- Click jenkins > manage jenkins > Credentials > System > Global credentials(unrestricted)
![9 global cred.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/a25b1df4-fdfa-4190-a5c0-32e588341e3a)


- Click add Credentials button
  - kind -> Username with password Username -> natasha  password -> Bl@kW ID -> ststor01

![10 new cred.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e6587789-993a-4840-b34b-c4e6d899089b)

-   Click create.

![11 natasha cred.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/35378480-a6ae-402d-acf3-c2eefe1e4612)

4.  **Add Manage node and cloud as per the task**

Click jenkins > manage jenkins > Manage Nodes and clouds >

![12 manage node cloud.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/b1ea29d1-4fc5-4738-aa15-f86a394187c2)

- Click New Node button

![13 nodes.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d18900f4-47ee-4b28-b5dc-197280cac7fa)

-   Node name -> Storage Server
    
-   Type -> Permanent agent


![14 new node.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/6815362c-015d-404a-a9df-89d4dca48e98)

-   Click create
```
Name -> Storage Server

Number of executors -> 1

Remote root directory - /var/www/html

labels : ststor01

launch method : only build jobs with label expression matching this node

Credentials -> choose natasha

Host key verification strategy -> non verify verfication strategy

Port -> 22
```


![15 create node.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ac347401-a756-418a-b545-c503bd425201)

-   Click save

-  Create node server
![16 add server node.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/50ff1671-9bfe-499e-ac33-5ad95ca451b8)

-   Check the launch agent for Storage Server
![17 lanuch agent.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3e4e7bfe-432f-4d77-9787-33276253a5f1)

-   Back to session terminal

```
thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:M79XmAej1mZBhIJMaUJ563dJqkvGCrDHnqA8dnuq8xQ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
natasha@ststor01's password: Bl@kW
[natasha@ststor01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for natasha: Bl@kW
[root@ststor01 ~]# 
[root@ststor01 ~]# yum install -y java-17-openjdk

[root@ststor01 ~]# java --version
openjdk 17.0.8 2023-07-18 LTS
OpenJDK Runtime Environment (Red_Hat-17.0.8.0.7-1) (build 17.0.8+7-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-17.0.8.0.7-1) (build 17.0.8+7-LTS, mixed mode, sharing)
[root@ststor01 ~]# 

```
![6 java version.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/7195b53d-df85-4b2d-a476-fdb91d8ffef5)

- Log Error

![18 1 log error.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ddc926e9-46a5-4849-ba88-36be31c84f8b)

- Back to terminal 

```
[root@ststor01 ~]# cd /var/www
[root@ststor01 www]# ls
html
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 sarah sarah 4096 Oct 18 12:22 html
[root@ststor01 www]# chown -R natasha html/
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 natasha sarah 4096 Oct 18 12:22 html
[root@ststor01 www]# 
```
![19 log sucess.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/2264a49a-2666-4296-894e-2d6f112ba8cb)
-   Agent successfully connected and online

- Create Storage Server node and cloud:

![20 create server no error.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9f037dd0-5823-41d7-a90a-cd54107c6afe)


5.  **Apache is already installed on all app Servers its running on port  `8080`.**  
![21 8091 welcomepage.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4743c197-a4bf-4800-8f68-0d5ed2b758cd)


-  remove `index.html` file
```
[root@ststor01 www]# cd html/
[root@ststor01 html]# ls
index.html  remoting  remoting.jar
[root@ststor01 html]# rm index.html
rm: remove regular file 'index.html'? y
[root@ststor01 html]# 

```
- Refresh this page 
![22 test page.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/17a32dfb-8a0f-4d19-8904-c5fe5d9d1dd1)

6. **click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`.**


![24 gitea login.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/1224a2ba-fe57-4af9-997d-b064e41b4d67)

-   Click the repository named  `sarah/web-app`  in Gitea. copy the Git link

![25 webapp.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/2d77598c-c33d-46cd-a617-fc671ffa7c84)

7. Create a Jenkins pipeline job named  `devops-webapp-job`  (it must not be a  `Multibranch pipeline`) and configure it to: 

![23 create job.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e4ebf85a-c3eb-4ff5-98b9-e7b56fd61f18)
- Click ok.

- Create the pipeline script.

```
pipeline {
    agent {
        label 'ststor01'
    }
    
    stages {
        stage('Deploy') {
            steps {
                git branch: "master",
                    url: "http://git.stratos.xfusioncorp.com/sarah/web_app.git"
                
                sh "cp -r /var/www/html/workspace/devops-webapp-job/* /var/www/html/"
            }
        }
    }
}
```


![26 pipeline.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/df5c4282-d89c-415c-8427-957312fe0316)

-  Create `devops-webapp-job`
![27 create pipeline.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0c9e4852-bde2-43d9-aac9-951f0edc2282)

- Build now

![28 build suces.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4ff40921-8480-42cc-b5ce-d2846a75b99a)


-  Console Output

![30 build sucess.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/927b4e1f-d999-481b-837d-c56072fc01c0)

- Refresh the App Page
![29 test page build .PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/8ae6e130-990e-41fb-9c00-ff35c19fd9ed)

![31 complete.PNG](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/7362de77-9980-4b7d-918b-975fd4d9ae09)

8. **Complete the project.**