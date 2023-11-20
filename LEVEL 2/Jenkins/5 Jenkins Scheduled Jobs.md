

## Questions:

The devops team of xFusionCorp Industries is working on to setup centralised logging management system to maintain and analyse server logs easily. Since it will take some time to implement, they wanted to gather some server logs on a regular basis. At least one of the app servers is having issues with the Apache server. The team needs Apache logs so that they can identify and troubleshoot the issues easily if they arise. So they decided to create a Jenkins job to collect logs from the server. Please create/configure a Jenkins job as per details mentioned below:



- Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n32`

1. Create a Jenkins jobs named `copy-logs`.

2. Configure it to periodically `build every 6 minutes` to copy the Apache logs `both access_log and error_logs` from `App Server 3`from default logs location to location `/usr/src/sysops` on `Storage Server`.

Note:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e update centre. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

2. Please make sure to define you cron expression like this `*/10 * * * *` (this is just an example to run job every 10 minutes).

3. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



## Solution: 

- Terminal-1:

```
thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n


[root@stapp03 ~]# cd /var/
[root@stapp03 var]# ls
adm    db     ftp    gopher    lib    lock  mail  opt       run    tmp  yp
cache  empty  games  kerberos  local  log   nis   preserve  spool  www
[root@stapp03 var]# cd log/
[root@stapp03 log]# ls
btmp  dnf.librepo.log  dnf.log  dnf.rpm.log  hawkey.log  httpd  lastlog  private  rhsm  wtmp
[root@stapp03 log]# cd httpd/
[root@stapp03 httpd]# ls
access_log  error_log
[root@stapp03 httpd]# exit
logout

[banner@stapp03 ~]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/banner/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/banner/.ssh/id_rsa.
Your public key has been saved in /home/banner/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:SRiLmbPc4qsD0C8n8QHfb0d5zdoXoYtHH9tqDhuFIeo banner@stapp03.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 3072]----+
|      .          |
|  .  + +       . |
| . o=.o . ...o. .|
|. o.o+.. oo..=oo |
|.  ++...S. .+o+ =|
|. o.+. .o ...+.oo|
| . +.  .E.  +  o |
|  .  .       +o  |
|  .o.       .o.  |
+----[SHA256]-----+
[banner@stapp03 ~]$ cd .ssh/
[banner@stapp03 .ssh]$ ls
id_rsa  id_rsa.pub  known_hosts

[banner@stapp03 .ssh]$ ssh-copy-id natasha@ststor01
natasha@ststor3's password:  Bl@kW

[banner@stapp03 .ssh]$ ssh natasha@ststor01
```


Last login: Thu Sep  7 10:30:45 2023 from 172.16.238.12
[natasha@ststor01 ~]$ 

```

[natasha@ststor01 ~]$ cd /usr/src/sysops
[natasha@ststor01 sysops]$ ls
access_log  error_log

after 1st build remove the all log file: 
[natasha@ststor01 sysops]$ rm *
[natasha@ststor01 sysops]$ ls

after second build see this log:
[natasha@ststor01 sysops]$ ls
access_log  error_log
```

- Click on the (Jenkins) button on the top bar to access the Jenkins UI

**1. Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8080 and click on Display Port.** 

- Able to access the Jenkins login page. Login using username admin and Adm!n321 password.


**2. Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.**

- Search for ssh , ssh credentials, ssh build agents  plugin and click Download now and install after restart


**3. In the following screen, click checkbox Restart Jenkins when installation is complete.**

- Login using username (admin) and password (Adm!n321).


**4. create the credentials setting for global:**

- manage jenkins-> credentials-> new credentials-> Kind = username with password -> scope= global(jenkins, nodes, item, all child items etc) ->

```
username : banner
password - BigGr33n
id- stapp03
```

**5. Create the configure system ssh remote**

- Dashboard->Manage Jenkins-->Configure System
->ssh remote hosts-> ssh sites ->
```
hostname = stapp03
port = 22
credentials = banner
check connection -> Successfull connection
click save button.
```

**6. Create a Jenkins jobs named copy-logs.**

- copy-logs -> freestyle-> click ok button. 


- copy-logs->configure->

-  Build Triggers  -> build periodically-> schedule: */6 * * * *-> click save button.


- Build Steps -> choose Execute shell script on remote host using ssh:

```
ssh site : banner@stapp03:22:22
command : scp /var/log/httpd/* natasha@ststor01:/usr/src/sysops
```

- build now-> 

# 1

```
Console Output
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/copy-logs
[SSH] script:

scp /var/log/httpd/* natasha@ststor01:/usr/src/sysops

[SSH] executing...

[SSH] completed
[SSH] exit-status: 0

Finished: SUCCESS
```

# 2 
```
Console Output
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/copy-logs
[SSH] script:

scp /var/log/httpd/* natasha@ststor01:/usr/src/sysops

[SSH] executing...

[SSH] completed
[SSH] exit-status: 0

Finished: SUCCESS
```

**7. Click on `Finish` & `Confirm` to complete the task successful**