

## Question:

Some of the developers from Nautilus project team have asked for SFTP access to at least one of the app server in Stratos DC. After going through the requirements, the system admins team has decided to configure the SFTP server on App Server 3 server in Stratos Datacenter. Please configure it as per the following instructions:



- a. Create an SFTP user james and set its password to BruCStnMT5.

- b. Password authentication should be enabled for this user.

- c. Set its ChrootDirectory to /var/www/data.

- d. SFTP user should only be allowed to make SFTP connections.


## Solution:  

**1. At first login on App server as per the task &  Switch to root user**

```

thor@jump_host ~$ ssh banner@stapp03
banner@stapp03's password: BigGr33n

[banner@stapp03 ~]$ sudo su -
[sudo] password for banner: BigGr33n
```

**2. Create the SFTP user  & set the password as per given in the task**

```

[root@stapp03 ~]# useradd james
[root@stapp03 ~]# passwd james
Changing password for user james.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```

**3. Make directory & set it as ChrootDirectory as per task**

```

[root@stapp03 ~]# mkdir -p /var/www/data
[root@stapp03 ~]# ll -lsd /var/www/data
[root@stapp03 ~]# chown root:root /var/www
[root@stapp03 ~]# chmod -R 755 /var/www
[root@stapp03 ~]# chmod -R 755 /var/www/data
[root@stapp03 ~]# chown -R james /var/www/data
[root@stapp03 ~]# chown -R root /var/www/data
[root@stapp03 ~]# chown -R root /var/www/
[root@stapp03 ~]# chmod -R 755 /var/www/
[root@stapp03 ~]# ll -lsd /var/www/
```

**4. Edit the  /etc/ssh/sshd_config  file and add the parameters as per below**
Note : - User & Chrootdirectory would differ so please check your task

```

[root@stapp03 ~]# vi /etc/ssh/sshd_config

open new terminal 
insert # subsystem
#Subsystem      sftp    /usr/libexec/openssh/sftp-server
then edit like this

Subsystem       sftp    internal-sftp

insert use -i then add below command

Match User james
ForceCommand internal-sftp
PasswordAuthentication yes
ChrootDirectory /var/www/data
PermitTunnel no
AllowTcpForwarding no
X11Forwarding no
AllowAgentForwarding no

control+c  exit :wq
```

**5. Restart the sshd   service** 

```
systemctl restart sshd
systemctl status sshd
```

**6.  Validate the task by below commands from the jump server as well localhost**

```

[root@stapp03 ~]# sftp james@localhost

Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
james@localhost's password: 
Connected to localhost.
sftp> exit
[root@stapp03 ~]# exit
logout
[banner@stapp03 ~]$ exit
logout
Connection to stapp03 closed.

thor@jump_host ~$ ssh james@stapp03
james@stapp03's password: 
This service allows sftp connections only.
Connection to stapp03 closed.

thor@jump_host ~$ sftp james@stapp03
james@stapp03's password: 
Connected to stapp03.
sftp> 
```

**7. Click on `Finish` & `Confirm` to complete the task successfully**

