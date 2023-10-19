

## Questions

The development team of xFusionCorp Industries is working on to develop a new static website and they are planning to deploy the same on Nautilus App Servers using Jenkins pipeline. They have shared their requirements with the DevOps team and accordingly we need to create a Jenkins pipeline job. Please find below more details about the task:  

Click on the  `Jenkins`  button on the top bar to access the Jenkins UI. Login using username  `admin`  and password  `Adm!n321`.  

Similarly, click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`. There under user  `sarah`  you will find a repository named  `web_app`  that is already cloned on  `Storage server`  under  `/var/www/html`. sarah is a developer who is working on this repository.  
  
1.  Add a slave node named  `Storage Server`. It should be labeled as  `ststor01`  and its remote root directory should be  `/var/www/html`.  
      
 2.  We have already cloned repository on  `Storage Server`  under  `/var/www/html`.  
      
    
3.  Apache is already installed on all app Servers its running on port  `8080`.  
      
  4.  Create a Jenkins pipeline job named  `nautilus-webapp-job`  (it must not be a  `Multibranch pipeline`) and configure it to:  
      -   Add a string parameter named  `BRANCH`.  
      
      -   It should conditionally deploy the code from  `web_app`  repository under  `/var/www/html`  on  `Storage Server`, as this location is already mounted to the document root  `/var/www/html`  of app servers. The pipeline should have a single stage named  `Deploy`  ( which is case sensitive ) to accomplish the deployment.  
      
        -   The pipeline should be conditional, if the value  `master`  is passed to the  `BRANCH`  parameter then it must deploy the  `master`  branch, on the other hand if the value  `feature`  is passed to the  `BRANCH`  parameter then it must deploy the  `feature`  branch.  
 
        
LB server is already configured. You should be able to see the latest changes you made by clicking on the  `App`  button. Please make sure the required content is loading on the main URL  `https://<LBR-URL>`  i.e there should not be a sub-directory like  `https://<LBR-URL>/web_app`  etc.  
  

`Note:`  
  

1.  You might need to install some plugins and restart Jenkins service. So, we recommend clicking on  `Restart Jenkins when installation is complete and no jobs are running`  on plugin installation/update page i.e  `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.  
      
    
2.  For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.




## Solution:

1.  **Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8081 and click on Display Port. You should be able to access the Jenkins login page. Login using username the  `admin`  and  `Adm!n321`  password.**

![2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/2eb7e076-fdde-4ba2-a713-eaf54f6f5abc)


![3](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/a221a9f3-367d-408e-867f-1e5f5dde7f58)

2.  **Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.**
![4](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4305cc82-5dc5-41f1-8027-c25e8cbb3b33)

-   Search for ssh , credentials, pipeline & git -> select all plugin checkbox click and click Download now and install after restart.
![5](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/48b7ae64-183a-4935-b929-59fe61a950cd)

-   click Download now and install after restart.

![6](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/bec5e0ae-97d8-4820-bffc-1e1922bc7b20)

![7](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/34e283c3-bf85-491c-963d-2b8592d2f2f7)

-   Restart jenkins
3.  **Click jenkins > manage jenkins > Credentials**
![9](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/77ac5024-393d-4bbd-8bdd-f6808482eb30)

-   Click credentials

- Click jenkins > manage jenkins > Credentials > System
![10](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/258c34cc-ba53-4567-b4be-04a016b07462)

- Click jenkins > manage jenkins > Credentials > System > Global credentials(unrestricted)
![11](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c9955de6-4416-41f8-8145-4ae48d169397)

![12](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/87cbc0c3-af09-460d-a8b9-f0f7100b9a89)

-   Click add Credentials button
    
    -   kind -> Username with password Username -> natasha password -> Bl@kW ID -> ststor01
![13](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/08890941-46b6-414b-9c44-95cd1480b08d)

-   Click create.
![14](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/58bd1e35-4422-44b6-ba9f-a8cc109db854)

4.  **Add Manage node and cloud as per the task**

Click jenkins > manage jenkins > Manage Nodes and clouds >
![15](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ac418d88-db77-4e54-afce-a5dd5a61e808)

-   Click New Node button
![16](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/df3c3ea2-fb41-4b47-bafb-a0b78ff69bc4)

- Click node
-   Node name -> Storage Server
    
-   Type -> Permanent agent
![17](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0d09e1f3-eb04-4362-a130-f3e41a4e2615)

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
![18](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/8bccf9dd-3a57-429b-8932-4ffc27f366c2)

- Click save.

- Create node server
![19](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/592aceee-02bd-4e15-9411-cdfcf1814e1a)

-   Back to session terminal.
```
thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:RCfpKVWBVOP2inRbO6D/NWEFCpPt1blmEnLRyo4wpsY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
[natasha@ststor01's]$ password: Bl@kW

[natasha@ststor01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:
    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.
[sudo] password for natasha: Bl@kW
[natasha@ststor01 ~]$ 

[root@ststor01 ~]# yum install java-17-openjdk -y

[root@ststor01 ~]# java --version
openjdk 17.0.9 2023-10-17 LTS
OpenJDK Runtime Environment (Red_Hat-17.0.9.0.9-1) (build 17.0.9+9-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-17.0.9.0.9-1) (build 17.0.9+9-LTS, mixed mode, sharing)
[root@ststor01 ~]# 

```
![8 java](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/af57450d-bc83-41cb-bc4e-09f0cead755e)

```
[root@ststor01 ~]# cd /var/www
[root@ststor01 www]# ls
html
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 sarah sarah 4096 Oct 19 15:01 html
[root@ststor01 www]# chown -R natasha html/
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 natasha sarah 4096 Oct 19 15:01 html
[root@ststor01 www]# 
```
- Back to jenkins.
-   Log 
![21](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d134857d-4f18-45cd-8bf8-662709dcc58c)

-   Agent successfully connected and online

- Check the launch agent for Storage Server up:

![20](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/bd0519ef-b3c4-471d-b2df-8b9d98efae3c)

5. **Apache is already installed on all app Servers its running on port  `8080`.**

![28](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/16e0adb5-2ae5-4360-aec6-51ee1cb8c02b)

6.  Create a Jenkins pipeline job named  `nautilus-webapp-job`  (it must not be a  `Multibranch pipeline`) and configure it to: 

![22](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/dc24d9ac-1094-474a-ac07-f5639bf085db)

![23](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/097208db-c2c3-421c-8002-fd192fd971a3)

-   Click ok.
    
-   Create the pipeline script.

```
pipeline {
    agent {
        label 'ststor01'
    }
    
    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to deploy (master or feature)')
    }
    
    stages {
        stage('Deploy') {
            when {
                expression {
                    params.BRANCH == 'master' || params.BRANCH == 'feature'
                }
            }
            steps {
                script {
                    def repositoryPath = '/var/www/html/'

                    if (params.BRANCH == 'master') {
                        git branch: 'master',
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    } else if (params.BRANCH == 'feature') {
                        git branch: 'feature',
                            url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    }
                    
                    sh "cp -r /var/www/html/workspace/xfusion-webapp-job/* /var/www/html/"
            }
                }
            }
        }
    }
```


![27](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/6a8f0f05-506e-4ea6-9a11-426b8c4a17e7)

- Click save.

- Click Build Now
![29](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0757cbc7-fd52-4cc0-b3dc-4daa2f500b44)


- Console Outupt:

```
Console Output
Started by user admin
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Storage Server in /var/www/html/workspace/xfusion-webapp-job
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] script
[Pipeline] {
[Pipeline] git
The recommended git tool is: NONE
No credentials specified
Cloning the remote Git repository
Cloning repository http://git.stratos.xfusioncorp.com/sarah/web_app.git
 > git init /var/www/html/workspace/xfusion-webapp-job # timeout=10
Fetching upstream changes from http://git.stratos.xfusioncorp.com/sarah/web_app.git
 > git --version # timeout=10
 > git --version # 'git version 2.39.1'
 > git fetch --tags --force --progress -- http://git.stratos.xfusioncorp.com/sarah/web_app.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url http://git.stratos.xfusioncorp.com/sarah/web_app.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
Checking out Revision 2e764e971d4c73ed893f619f4b0b5d96b0a3d70e (refs/remotes/origin/master)
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 2e764e971d4c73ed893f619f4b0b5d96b0a3d70e # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git checkout -b master 2e764e971d4c73ed893f619f4b0b5d96b0a3d70e # timeout=10
Commit message: "Added index.html file"
First time build. Skipping changelog.
[Pipeline] sh
+ cp -r /var/www/html/workspace/xfusion-webapp-job/index.html /var/www/html/
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```

![30](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/135140f4-fc13-455d-bbcb-92ebea7b1f92)
![31](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/73f5e385-2ad2-47ae-9b92-af91da3d0558)

![33](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/999e8402-0699-478a-b041-a206611dc782)

-output without parameter:
![32](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/caaae1f1-144b-4351-beca-ed3c300bafe2)

- Add parameter automatic after build:

![34](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/2c6b7ed2-38c6-453c-8ec9-9ffa65830d7c)

- Click Build with parameter:
![35](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/fc040c0c-16ac-4672-829b-e3ebf5440a05)


-Output:
![32](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/caaae1f1-144b-4351-beca-ed3c300bafe2)

![38 feature build](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0d85d358-54dd-4773-a190-24703dc17bcb)


![39 master](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0a9b2227-9f98-4e89-948d-a3e9712cbf37)

- If change Branch : feature
- Output:
![28](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/16e0adb5-2ae5-4360-aec6-51ee1cb8c02b)

- If again change Branch : master
 ![35](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/fc040c0c-16ac-4672-829b-e3ebf5440a05)
- output:
![36](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0e6e0333-e5bf-4e02-b304-60fb555ac84d)

7.  **click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`.**

![25](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/86230a2b-3528-48a0-91b8-2aad4b243f76)


-   Click the repository named  `sarah/web-app`  in Gitea. copy the Git link

![26](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0f1ca38a-2f22-4415-8614-5d07a2e91d4a)


![41 gitea output](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/f0090992-2348-4392-ab05-cd47b7c4a942)

![36](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0e6e0333-e5bf-4e02-b304-60fb555ac84d)


![42 gitea feature](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/912c81d8-14c4-460b-b2b8-3c92d172703b)

![28](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/16e0adb5-2ae5-4360-aec6-51ee1cb8c02b)

-Build Success.
![43](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/6f42f6d0-300b-4cd7-831c-477ceb59f04a)

8. **Complete the task:**
![44](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/56292aed-574c-4ede-92f7-9fa67aeee87a)

