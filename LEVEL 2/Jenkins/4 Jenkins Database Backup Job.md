

## Questions:

There is a requirement to create a Jenkins job to automate the database backup. Below you can find more details to accomplish this task:



-Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.


- Create a Jenkins job named `database-backup`.


Configure it to take a database dump of the `kodekloud_db01` database present on the `Database server in Stratos Datacenter`, the database user is `kodekloud_roy` and password is `asdfgdsd`.


The dump should be named in `db_$(date +%F).sql` format, where `date +%F` is the current date.

Copy the `db_$(date +%F).sql` dump to the `Backup Server` under location `/home/clint/db_backups`.


Further, schedule this job to run periodically at `*/10 * * * *` (please use this exact schedule format).


Note:

You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.


Please make sure to define you cron expression like this `*/10 * * * *` `this is just an example to run job every 10 minutes`.


For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



## Solution: 

- terminal-1:

```

thor@jump_host ~$ ssh peter@stdb01
The authenticity of host 'stdb01 (172.16.239.10)' can't be established.
ECDSA key fingerprint is SHA256:uVNCGXLNAIintdwwtAYiHYlCAdFgoYZexTeEfWS7kxg.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stdb01,172.16.239.10' (ECDSA) to the list of known hosts.
peter@stdb01's password: Sp!dy

[peter@stdb01 ~]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/peter/.ssh/id_rsa): 
Created directory '/home/peter/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/peter/.ssh/id_rsa.
Your public key has been saved in /home/peter/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:cJGqoJLprGhShjCSjuzW4xojj9BK1SlrXTEzVZycEkc peter@stdb01.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 3072]----+
|        .+*Eo    |
|        oo.=     |
| .    .*. .      |
|= . . oo=        |
|** + + .S        |
|B++ = .          |
|B*oo .           |
|*X+o             |
|X.+..            |
+----[SHA256]-----+
```

- Backup Server

```

[peter@stdb01 ~]$ ssh-copy-id clint@stbkp01
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/peter/.ssh/id_rsa.pub"
The authenticity of host 'stbkp01 (172.17.0.5)' can't be established.
ECDSA key fingerprint is SHA256:82wk7nwgRJOf1gBHty7ZdNPEH8gW0VmBMrNCyza0qjY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? YES
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
clint@stbkp01's password: H@wk3y3
```


- Number of key(s) added: 1

- Now try logging into the machine, with:   "ssh 'clint@stbkp01'"
and check to make sure that only the key(s) you wanted were added.

```

[peter@stdb01 ~]$ ssh clint@stbkp01
[clint@stbkp01 ~]$ 
cd 
logout
Connection to stbkp01 closed.
[peter@stdb01 ~]$ ls
[peter@stdb01 ~]$ pwd
/home/peter
[peter@stdb01 ~]$ ls
[peter@stdb01 ~]$ ssh clint@stbkp01
Last login: Wed Sep  6 11:43:16 2023 from 172.17.0.8
[clint@stbkp01 ~]$ ls
db_backups
[clint@stbkp01 ~]$ cd db_backups

[clint@stbkp01 db_backups]$ ls
db_2023-09-06.sql 
```

- apt-get install libmysqlclient-dev

- Click on the `Jenkins` button on the top bar to access the Jenkins UI

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
password - Sp!dy
id- db_creds
```

**5. Create the configure system ssh remote**

- Dashboard->Manage Jenkins-->Configure System
->ssh remote hosts-> ssh sites ->
```
hostname = stdb01
port = 22
credentials = peter 
check connection -> Successfull connection
click save button.
```

**6. Create a Jenkins job named database-backup.**

- database-backup -> freestyle-> click ok button. 

- Build Steps -> choose Execute shell script on remote host using ssh:

```
ssh site : peter@stb01:22
command :  mysqldumb -u kodekloud_roy -pasdfgdsd kodekloud_db01 >db_$(date +%F).sql

scp -o StrictHostKeyChecking=no db_$(date +%F).sql clint@stbkp01:/home/clint/db_backups/

Build Triggers  -> build periodically-> schedule: */10 * * * * -> click save button.
```

- build now-> 

#1 
```
Console Output
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/database-backup
[SSH] script:

mysqldumb -u kodekloud_roy -pasdfgdsd kodekloud_db01 >db_$(date +%F).sql

scp -o StrictHostKeyChecking=no db_$(date +%F).sql clint@stbkp01:/home/clint/db_backups/

[SSH] executing...
bash: line 1: mysqldumb: command not found

[SSH] completed
[SSH] exit-status: 0

Finished: SUCCESS
```

**6. Click on `Finish` & `Confirm` to complete the task successful**