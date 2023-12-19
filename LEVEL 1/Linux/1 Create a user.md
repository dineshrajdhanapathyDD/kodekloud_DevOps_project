## Questions: 
For security reasons, the xFusionCorp Industries security
team has decided to use custom Apache users for each web application
hosted, rather than its default user. This will be the Apache user, so
it shouldn\'t use the default home directory. Create the user as per the
requirements given below:**

*a\. Create a user named `anita` on the App Server 2 in Stratos Datacenter.*

*b\. Set its UID to `1075` and home directory to `/var/www/anita`.*

## Solution: 
**1. Login on app server as per the task & Switch to root user**

```
thor@jump_host \~\$ ssh steve@stapp02 steve@stapp02\'s
password: Am3ric@ 
[steve@stapp02 \~\]$ sudo su -
[sudo] password for steve: Am3ric@
```

**2. Run the Below command to create a new user yousuf with UID**

```
[root@stapp02 \~\]# useradd -u 1075 anita
[root@stapp02 \~\]# id anita
uid=1075(anita) gid=1075(anita) groups=1075(anita)
[root@stapp02 \~\]# cat /etc/passwd \|grep anita anita:x:1075:1075::/home/anita:/bin/bash
```

**3. Post user creation run Below command to change user home directory.**

```
[root@stapp02 \~\]# usermod -d /var/www/anita -m anita
[root@stapp02 \~\]# cat /etc/passwd \|grep anita
anita:x:1075:1075::/var/www/anita:/bin/bash
```

**4. Click on `Finish` & `Confirm` to complete the task successfully.**
