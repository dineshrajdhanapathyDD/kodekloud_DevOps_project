
Questions:
The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 3 in Stratos Datacenter, and they need to create a bash script named beta_backup.sh which should accomplish the following tasks. (Also remember to place the script under /scripts directory on App Server 3)



a. Create a zip archive named xfusioncorp_beta.zip of /var/www/html/beta directory.

b. Save the archive in /backup/ on App Server 3. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.

c. Copy the created archive to Nautilus Backup Server server in /backup/ location.

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.

Solution : 
1. At first login to app as per your task
thor@jump_host ~$ ssh banner@stapp03
banner@stapp03's password: BigGr33n

2. Then go to the scripts folder and create a backup script 

    (Note - Please change the file name and folder as per your task)

[banner@stapp03 ~]$ vi /scripts/beta_backup.sh

open another terminal
use this code:
#!/bin/bash

zip -r /backup/xfusioncorp_beta.zip /var/www/html/beta

scp /backup/xfusioncorp_beta.zip clint@stbkp01:/backup
control+c enter :wq exit

[banner@stapp03 ~]$ cat  /scripts/beta_backup.sh
#!/bin/bash

zip -r /backup/xfusioncorp_beta.zip /var/www/html/beta

scp /backup/xfusioncorp_beta.zip clint@stbkp01:/backup

3. Then create a keygen and copy the key to the backup server so that the bash script will not require any password  

[banner@stapp03 ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/banner/.ssh/id_rsa):

use enter button for created directory '/home/banner/.ssh'/

4. Copy the key on the backup server & cross-check login without password prompt

[banner@stapp03 ~]$ ssh-copy-id clint@stbkp01

clint@stbkp01's password: H@wk3y3

[banner@stapp03 ~]$ ssh clint@stbkp01

[clint@stbkp01 ~]$ logout
Connection to stbkp01 closed.

5.  Finally go to scripts folder and run the bash scripts by this command

[banner@stapp03 ~]$ cd /scripts/

[banner@stapp03 scripts]$ sh beta_backup.sh


6. Now you can check the backup folder for the zip file on both app02 and backup server. If you find the zip file then it should be done.

[banner@stapp03 scripts]$ ll /backup
total 4

[banner@stapp03 scripts]$ ssh clint@stbkp01

[clint@stbkp01 ~]$ ll /backup
total 4


7. Click on Finish & Confirm to complete the task successful




