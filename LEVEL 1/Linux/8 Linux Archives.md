

## Questions:

On `Nautilus` storage server in `Stratos DC`, there is a storage location named `/data`, which is used by different developers to keep their data (non confidential data). One of the developers named `ravi` has raised a ticket and asked for a copy of their data present in `/data/ravi` directory on storage server. `/home` is a FTP location on storage server itself where developers can download their data. Below are the instructions shared by the system admin team to accomplish this task.

a. Make a `ravi.tar.gz` compressed archive of `/data/ravi` directory and move the archive to `/home` directory on Storage Server.


## Solution:  

**1. At first login on storage server  & Switch to  root user** 

```
thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW

[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**2. Run Below command to archive the folder**

```
[root@ststor01 ~]# ll /data
-bash: ll: command not found 

[root@ststor01 ~]# ls -l data/ravi
ls: cannot access 'data/ravi': No such file or directory
[root@ststor01 ~]# ls -l /data
total 4
drwxrwxrwx 2 root root 4096 Jul 25 16:33 ravi

[root@ststor01 ~]# tar -czvf ravi.tar.gz  /data/ravi/
tar: Removing leading `/' from member names
/data/ravi/
/data/ravi/nautilus2.txt
/data/ravi/nautilus3.txt
/data/ravi/nautilus1.txt
```

**3.  Move the  tar file to `/home`**

```
[root@ststor01 ~]# ls
anaconda-ks.cfg  anaconda-post-nochroot.log  anaconda-post.log  buildinfo  original-ks.cfg  ravi.tar.gz
[root@ststor01 ~]# ls -l
total 44
-rw------- 1 root root 9810 Feb  7 18:16 anaconda-ks.cfg
-rw-r--r-- 1 root root   95 Feb  7 18:16 anaconda-post-nochroot.log
-rw-r--r-- 1 root root 1083 Feb  7 18:16 anaconda-post.log
drwxr-xr-x 1 root root 4096 Feb  7 18:19 buildinfo
-rw------- 1 root root 9606 Feb  7 18:16 original-ks.cfg
-rw-r--r-- 1 root root  186 Jul 25 16:46 ravi.tar.gz

[root@ststor01 ~]# mv ravi.tar.gz  /home/
```

**4. Validate the task by listing the file exist in  Home directory**

```
[root@ststor01 ~]# ls -l /home/
total 12
drwx------ 1 ansible ansible 4096 Mar  6 06:19 ansible
drwx------ 1 natasha natasha 4096 Apr 11 08:53 natasha
-rw-r--r-- 1 root    root     186 Jul 25 16:46 ravi.tar.gz
```

**5. Click on `Finish` & `Confirm` to complete the task successful**
