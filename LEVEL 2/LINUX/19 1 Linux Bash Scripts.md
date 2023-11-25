

## Questions:

The production support team of xFusionCorp Industries is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for taking websites backup. They have a static website running on App Server 1 in Stratos Datacenter, and they need to create a bash script named media_backup.sh which should accomplish the following tasks. (Also remember to place the script under `/scripts` directory on App Server 1)



a. Create a zip archive named `xfusioncorp_media.zip` of `/var/www/html/media` directory.

b. Save the archive in `/backup/` on App Server 1. This is a temporary storage, as backups from this location will be clean on weekly basis. Therefore, we also need to save this backup archive on Nautilus Backup Server.

c. Copy the created archive to Nautilus Backup Server server in `/backup/` location.

d. Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, tony in case of App Server 1) must be able to run it.

## Solution :

**1. At first login to app as per your task**

```
tony@stapp01's password: Ir0nM@n
[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n
```

**2. Then go to the scripts folder and create a backup script**

```
    (Note - Please change the file name and folder as per your task)
[tony@stapp01 ~]$ vi /scripts/media_backup.sh
open another terminal paste the below sentnece
"#!/bin/bash
zip -r /backup/xfusioncorp_media.zip /var/www/html/media
scp /backup/xfusioncorp_media.zip clint@stbkp01:/backup"

[tony@stapp01 ~]$ cat  /scripts/media_backup.sh
#!/bin/bash
zip -r /backup/xfusioncorp_media.zip /var/www/html/media
scp /backup/xfusioncorp_media.zip clint@stbkp01:/backup
```

**3. Then create a keygen and copy the key to the backup server so that the bash script will not require any password**

```
[tony@stapp01 ~]$ ssh-keygen

"after asking confirmation just enter "
```

**4. Copy the key on the backup server & cross-check login without password prompt**

```
[tony@stapp01 ~]$ ssh-copy-id clint@stbkp01
clint@stbkp01's password: H@wk3y3

[tony@stapp01 ~]$ ssh clint@stbkp01
[clint@stbkp01 ~]$ logout
Connection to stbkp01 closed.
```

**5.  Finally go to scripts folder and run the bash scripts by this command**

```
[tony@stapp01 ~]$ cd /scripts/
[tony@stapp01 scripts]$ ll
total 4
-rw-rw-r-- 1 tony tony 125 Jul  3 11:30 media_backup.sh
[tony@stapp01 scripts]$ chmod +x media_backup.sh
[tony@stapp01 scripts]$ ll
total 4
-rwxrwxr-x 1 tony tony 125 Jul  3 11:30 media_backup.sh
[tony@stapp01 scripts]$ sh media_backup.sh
  adding: var/www/html/media/ (stored 0%)
  adding: var/www/html/media/index.html (stored 0%)
  adding: var/www/html/media/.gitkeep (stored 0%)
xfusioncorp_media.zip                             100%  595     1.4MB/s   00:00 
```

**6. Now you can check the backup folder for the zip file on both app01 and backup server. If you find the zip file then it should be done.**

```
[tony@stapp01 scripts]$ ll /backup
total 4
-rw-rw-r-- 1 tony tony 595 Jul  3 11:44 xfusioncorp_media.zip
[tony@stapp01 scripts]$ ssh clint@stbkp01
total 4
-rw-rw-r-- 1 clint clint 595 Jul  3 11:44 xfusioncorp_media.zip
```

**7. Click on `Finish` & `Confirm` to complete the task successful**
