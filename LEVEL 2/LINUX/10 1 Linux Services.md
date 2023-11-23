

## Questions:

As per details shared by the development team, the new application release has some dependencies on the back end. There are some packages/services that need to be installed on all app servers under Stratos Datacenter. As per requirements please perform the following steps:



a. Install httpd package on all the application servers.

b. Once installed, make sure it is enabled to start during boot.


## Solution: 

**1. At first login on storage server  ssh tony@stapp01**

```

thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n

```

**2. Switch to  root user : sudo su -**

```
[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n
```

**3. Run Below command to install IPtables service** 

```
    [root@stapp01 ~]# yum install  postfix -y
```

**4. Start & enable the squid service below commands**

```
[root@stapp01 ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:httpd(8)
           man:apachectl(8)


[root@stapp01 ~]# systemctl start httpd
[root@stapp01 ~]# 
[root@stapp01 ~]# systemctl enable httpd
Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.
```

**5. Validate the squid service status   systemctl status squid**

```
[root@stapp01 ~]# systemctl status httpd

Please Note :- I have showed only for stapp01. You have to do this in all app server stapp01,stapp02, stapp03. 
```

**6.  Click on `Finish` & `Confirm` to complete the task successful**

