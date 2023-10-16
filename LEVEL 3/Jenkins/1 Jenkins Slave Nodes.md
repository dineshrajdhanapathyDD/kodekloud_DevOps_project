
**Questions:**

The Nautilus DevOps team has installed and configured new Jenkins server in Stratos DC which they will use for CI/CD and for some automation tasks. There is a requirement to add all app servers as slave nodes in Jenkins so that they can perform tasks on these servers using Jenkins. Find below more details and accomplish the task accordingly.  

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `(Adm!n321)`.

1. Add all app servers as SSH build agent/slave nodes in Jenkins. Slave node name for `(app server 1)`, `(app server 2)` and `(app server 3)` must be `(App_server_1)`, `(App_server_2)`, `(App_server_3)` respectively.

 2. Add labels as below:
 
`(App_server_1 : stapp01)`

  

`(App_server_2 : stapp02)`

  

`(App_server_3 : stapp03)`
  
3. Remote root directory for `(App_server_1)` must be `(/home/tony/jenkins)`, for `(App_server_2) `must be `(/home/steve/jenkins)` and for `(App_server_3)` must be `(/home/banner/jenkins)`.

4. Make sure slave nodes are online and working properly.
 
`(Note)`:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `(Restart Jenkins when installation is complete and no jobs are running)` on plugin installation/update page i.e `(update centre)`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.
  


## Solution:

 
1. Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8081 and click on Display Port. You should be able to access the Jenkins login page. Login using username theadmin and Adm!n321 password.

  
![1 login page](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/efba7e02-b913-47ae-b30d-552ecdc84aa4)
 

2. Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.

Search for SSH -> select SSH Build Agents plugin and click Download now and install after restart

  

![2 ssh select](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/f902ed2b-d4dd-48c6-965a-a8c80da4cda4)

  

![3 Download progress](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c61ee3ed-0b27-4f0b-a87c-b85a905f35b7)

 
3. Click jenkins > manae jenkins > Credentials > System > Global credentials(unrestricted)

Click add Credentials button

kind -> Username with password

Username -> tony

password -> Ir0nM@n

ID -> stapp01

Click create

  

![4 tony credential](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/834d2c29-7d6a-4ff3-9cdd-377680f5860a)

  

Click add Credentials button

kind -> Username with password

Username -> steve

password -> Am3ric@

ID -> stapp02

Click create

  

![5 steve credential](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c9b331a7-9699-4a14-9b8c-36a728d821e0)

  

Click add Credentials button

kind -> Username with password

Username -> banner

password -> BigGr33n

ID -> stapp0

Click create

  

![6 banner credential](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/478f0348-5669-41b8-8038-3c6223bba9ee)

  

![7 global credential for 3 server](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/5994e5af-7d65-4747-bee9-64881d9d6a4e)

  
  

4. Add Slave Nodes as per the task

  

Click jenkins > manae jenkins > Manage Nodes and clouds >

  

click New Node button

Node name -> App_server_1

Type -> Permanent agent

Click create

  

![8 node app server1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ee603da1-8868-4fd0-816e-9f35e5d4b200)

  

Name -> App_server_1

Number of executors -> 1

Remote root directory - /home/tony/jenkins

labels : App_server_1 : stapp01

launch method : launch agents via ssh

Credentials -> choose tony

Host key verification strategy -> non verify verfication strategy

Port -> 22

Click save





![9 node create appserver1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/bef2e30d-a4bd-4f19-b68a-42918736bbc5)

  

![10 manage nodes and clouds server1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9ac04c6c-9fce-4359-b6f9-c7b4fab84cbb)

  

Check the launh agent for App_server_1 > log - java version issue

  

Back to session terminal

  

```
thor@jump_host ~$ ssh tony@stapp01

tony@stapp01's password: Ir0nM@n
 
[tony@stapp01 ~]$ sudo su -

[sudo] password for tony: Ir0nM@n

[root@stapp01 ~]# java --version

-bash: java: command not found
  
[root@stapp01 ~]# yum search java*

[root@stapp01 ~]# yum install java-17-openjdk-src.x86_64 -y

[root@stapp01 ~]# java --version

openjdk 17.0.6-ea 2023-01-17 LTS

OpenJDK Runtime Environment (Red_Hat-17.0.6.0.9-0.3.ea.el8) (build 17.0.6-ea+9-LTS)

OpenJDK 64-Bit Server VM (Red_Hat-17.0.6.0.9-0.3.ea.el8) (build 17.0.6-ea+9-LTS, mixed mode, sharing)

[root@stapp01 ~]#
```
  
  

![11 stapp01 java version](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3ce3dbc9-7628-4981-a37b-3946c9c7a834)

  

![12 node server1 log](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4b0e6578-63c7-4f85-908e-954ae8c5d354)

  

Agent successfully connected and online

  

click New Node button

Node name -> App_server_2

Type -> Permanent agent

Click create

  

![13 node app server2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ab4ad471-c327-473d-bf7a-09c8371fef97)

  

Name -> App_server_2

Number of executors -> 1

Remote root directory - /home/steve/jenkins

labels : App_server_2 : stapp02

launch method : launch agents via ssh

Credentials -> choose steve

Host key verification strategy -> non verify verfication strategy

Port -> 22

Click save

  

![14 node create appserver2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3ad7f57d-4b6b-4ff5-83b8-ae85ed78fe18)

  

Back to session terminal

  
```
thor@jump_host ~$ ssh steve@stapp02

tony@stapp02's password: Am3ric@
  
[tony@stapp02 ~]$ sudo su -

[sudo] password for tony: Am3ric@

[root@stapp02 ~]# java --version

-bash: java: command not found

[root@stapp02 ~]# yum search java*

[root@stapp02 ~]# yum install java-17-openjdk-src.x86_64 -y
  
[root@stapp02 ~]# java --version
openjdk 17.0.6-ea 2023-01-17 LTS
OpenJDK Runtime Environment (Red_Hat-17.0.6.0.9-0.3.ea.el8) (build 17.0.6-ea+9-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-17.0.6.0.9-0.3.ea.el8) (build 17.0.6-ea+9-LTS, mixed mode, sharing)
[root@stapp01 ~]#
```

  

![15 stapp02 java version](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/28947d15-9eee-48a9-a702-c0b935162213)

  

click New Node button

Node name -> App_server_3

Type -> Permanent agent

Click create

  

![17 node app server3](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/467c4255-4429-4e0f-8f73-1ddc1493eafd)

  

Name -> App_server_3

Number of executors -> 1

Remote root directory - /home/banner/jenkins

labels : App_server_3 : stapp03

launch method : launch agents via ssh

Credentials -> choose banner

Host key verification strategy -> non verify verfication strategy

Port -> 22

Click save

  

![18 node create app server3](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/73f6173c-3b89-40e6-8eb0-2af232c0a891)

  

Back to session terminal

  
```
thor@jump_host ~$ ssh banner@stapp03

tony@stapp03's password: BigGr33n

[tony@stapp03 ~]$ sudo su -

[sudo] password for tony: BigGr33n

[root@stapp03 ~]# java --version

-bash: java: command not found
  
[root@stapp03 ~]# yum search java*

[root@stapp03 ~]# yum install java-17-openjdk-src.x86_64 -y

[root@stapp03 ~]# java --version
openjdk 17.0.6-ea 2023-01-17 LTS
OpenJDK Runtime Environment (Red_Hat-17.0.6.0.9-0.3.ea.el8) (build 17.0.6-ea+9-LTS)
OpenJDK 64-Bit Server VM (Red_Hat-17.0.6.0.9-0.3.ea.el8) (build 17.0.6-ea+9-LTS, mixed mode, sharing)
[root@stapp01 ~]#
```
  

![19 stapp03 java version](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4c238cca-7d95-4788-b0a6-36e73e407ca3)

  
  

![20 manage node and clouds for 3 server ](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/175c83cd-593a-4d3c-8757-60247efef1f5)

  
  

5. Jenkins > New item >

  

Create item name

name -> test-job

choose freestyle project

click ok

  

![21 test job](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/18854ed8-a2be-4142-93b0-b14242950841)

  
  

6. jenkins > test-job > configuration

  

Build steps

Choose execte shell -> echo 'hello'

click save

  

![22 execute shell command](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/681b3425-1184-4b28-aafc-19818b5ba800)

  

Console output for App_server_1

  

![23 appserver1 output](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4ecd2a80-0648-40f2-a662-1ec777da41df)

  

Console output for App_server_2

  
  

![24 appserver2 output](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/2742811f-a338-4c58-a3d1-8ed793d2c60b)

  

Console output for App_server_2

  
  

![25 appserver3 output ](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/64c94354-7eee-4ca4-af78-8bc3d2aca73a)

  
  

7. Click on Finish & Confirm to complete the task successful