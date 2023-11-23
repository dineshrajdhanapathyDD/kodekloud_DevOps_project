

## Questions:

We have some users on all app servers in Stratos Datacenter. Some of them have been assigned some new roles and responsibilities, therefore their users need to be upgraded with sudo access so that they can perform admin level tasks.



a. Provide sudo access to user mark on all app servers.

b. Make sure you have set up password-less sudo for the user.


## Solution:  

**1. Login on all app servers & switch to root user**

```
thor@jump_host ~$ ssh tony@stapp01

tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n
```

**2. Check user is existing & have sudo permission**

```
[root@stapp01 ~]# id mark

[root@stapp01 ~]# su - mark

[mark@stapp01 ~]$ sudo cat /etc/sudoers |grep mark

[sudo] password for mark : dont enter password
```

**3. switch to root and provide sudo permission without a password**

```
[mark@stapp01 ~]$ logout

[root@stapp01 ~]# visudo
enter another terminal
insert -i o edit below

ansible    ALL=(ALL)   NOPASSWD:ALL
mark    ALL=(ALL)   NOPASSWD:ALL
 exit :wq

[root@stapp01 ~]# su - mark

[mark@stapp01 ~]$ sudo cat /etc/sudoers |grep mark
mark    ALL=(ALL)   NOPASSWD:ALL

Please Note:- I have shown only for stapp01. 
You have to do this in all app server stapp01-Ir0nM@n,stapp02 - Am3ric@, stapp03 - BigGr33n.
```

**4.  Click on `Finish` & `Confirm` to complete the task successfully**