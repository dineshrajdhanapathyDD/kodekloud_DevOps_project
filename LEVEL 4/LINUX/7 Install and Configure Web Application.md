

## Question

xFusionCorp Industries is planning to host two static websites on their infra in  `Stratos Datacenter`. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task:  
  

  

a. Install  `httpd`  package and dependencies on  `app server 2`.  
  

b. Apache should serve on port  `8087`.  
  

c. There are two website's backups  `/home/thor/official`  and  `/home/thor/apps`  on  `jump_host`. Set them up on Apache in a way that  `official`  should work on the link  `http://localhost:8087/official/`  and  `apps`  should work on link  `http://localhost:8087/apps/`  on the mentioned app server.  
  

d. Once configured you should be able to access the website using  `curl`  command on the respective app server, i.e  `curl http://localhost:8087/official/`  and  `curl http://localhost:8087/apps/`


## solution

**1. At first login on app server  `ssh steve@stapp02` & Switch to root user : `sudo su -`**

```

thor@jump_host ~$ ssh steve@stapp02
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:Jip090J84zztTxPNi7YYDsj8yvLI1HXhKzPWMxLBo48.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02,172.16.238.11' (ECDSA) to the list of known hosts.
steve@stapp02's password: Am3ric@

[steve@stapp02 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: Am3ric@

[root@stapp02 ~]# 
```
**2. Run Below command to install openssh service for copying scp files**

```

[root@stapp02 ~]# yum install openssh-clients -y
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                              2.5 kB/s | 4.4 kB     00:01    
CentOS Stream 8 - AppStream                                              9.3 MB/s |  34 MB     00:03    
CentOS Stream 8 - BaseOS                                                  14 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                  40 MB/s |  52 MB     00:01    
CentOS Stream 8 - Extras                                                 6.7 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                  14 kB/s | 3.0 kB     00:00    
CentOS Stream 8 - Extras common packages                                  25 kB/s | 6.9 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                           2.5 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                         15 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                733 kB/s | 100 kB     00:00    
Package openssh-clients-8.0p1-17.el8.x86_64 is already installed.
Dependencies resolved.
=========================================================================================================
 Package                   Architecture     Version                    Repository                   Size
=========================================================================================================
Upgrading:
 openssh                   x86_64           8.0p1-19.el8_8             ubi-8-baseos-rpms           524 k
 openssh-clients           x86_64           8.0p1-19.el8_8             ubi-8-baseos-rpms           669 k
 openssh-server            x86_64           8.0p1-19.el8_8             ubi-8-baseos-rpms           493 k

Transaction Summary
=========================================================================================================
Upgrade  3 Packages

Total download size: 1.6 M
Downloading Packages:
(1/3): openssh-server-8.0p1-19.el8_8.x86_64.rpm                          4.3 MB/s | 493 kB     00:00    
(2/3): openssh-clients-8.0p1-19.el8_8.x86_64.rpm                         5.5 MB/s | 669 kB     00:00    
(3/3): openssh-8.0p1-19.el8_8.x86_64.rpm                                 4.1 MB/s | 524 kB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                     12 MB/s | 1.6 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Running scriptlet: openssh-8.0p1-19.el8_8.x86_64                                                   1/1 
  Running scriptlet: openssh-8.0p1-19.el8_8.x86_64                                                   1/6 
  Upgrading        : openssh-8.0p1-19.el8_8.x86_64                                                   1/6 
  Upgrading        : openssh-clients-8.0p1-19.el8_8.x86_64                                           2/6 
  Running scriptlet: openssh-server-8.0p1-19.el8_8.x86_64                                            3/6 
  Upgrading        : openssh-server-8.0p1-19.el8_8.x86_64                                            3/6 
  Running scriptlet: openssh-server-8.0p1-19.el8_8.x86_64                                            3/6 
  Running scriptlet: openssh-server-8.0p1-17.el8.x86_64                                              4/6 
  Cleanup          : openssh-server-8.0p1-17.el8.x86_64                                              4/6 
  Running scriptlet: openssh-server-8.0p1-17.el8.x86_64                                              4/6 
  Cleanup          : openssh-clients-8.0p1-17.el8.x86_64                                             5/6 
  Cleanup          : openssh-8.0p1-17.el8.x86_64                                                     6/6 
  Running scriptlet: openssh-8.0p1-17.el8.x86_64                                                     6/6 
  Verifying        : openssh-8.0p1-19.el8_8.x86_64                                                   1/6 
  Verifying        : openssh-8.0p1-17.el8.x86_64                                                     2/6 
  Verifying        : openssh-clients-8.0p1-19.el8_8.x86_64                                           3/6 
  Verifying        : openssh-clients-8.0p1-17.el8.x86_64                                             4/6 
  Verifying        : openssh-server-8.0p1-19.el8_8.x86_64                                            5/6 
  Verifying        : openssh-server-8.0p1-17.el8.x86_64                                              6/6 
Installed products updated.

Upgraded:
  openssh-8.0p1-19.el8_8.x86_64                      openssh-clients-8.0p1-19.el8_8.x86_64              
  openssh-server-8.0p1-19.el8_8.x86_64              

Complete!
```
**3. Login back to app server `stapp02` & install `httpd`**
```

[root@stapp02 ~]# sudo yum install -y httpd

Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 0:00:44 ago on Sat Nov  4 12:11:42 2023.
Dependencies resolved.
=========================================================================================================
 Package                  Architecture Version                                     Repository       Size
=========================================================================================================
Installing:
 httpd                    x86_64       2.4.37-62.module_el8+657+88b2113f           appstream       1.4 M
Installing dependencies:
 apr                      x86_64       1.6.3-12.el8                                appstream       129 k
 apr-util                 x86_64       1.6.1-9.el8                                 appstream       106 k
 centos-logos-httpd       noarch       85.8-2.el8                                  appstream        75 k
 httpd-filesystem         noarch       2.4.37-62.module_el8+657+88b2113f           appstream        44 k
 httpd-tools              x86_64       2.4.37-62.module_el8+657+88b2113f           appstream       111 k
 mailcap                  noarch       2.1.48-3.el8                                baseos           39 k
 mod_http2                x86_64       1.15.7-8.module_el8+452+6213a5e6.3          appstream       155 k
Installing weak dependencies:
 apr-util-bdb             x86_64       1.6.1-9.el8                                 appstream        25 k
 apr-util-openssl         x86_64       1.6.1-9.el8                                 appstream        27 k
Enabling module streams:
 httpd                                 2.4                                                              

Transaction Summary
=========================================================================================================
Install  10 Packages

Total download size: 2.1 M
Installed size: 5.6 M
Downloading Packages:
(1/10): apr-util-bdb-1.6.1-9.el8.x86_64.rpm                              132 kB/s |  25 kB     00:00    
(2/10): apr-util-openssl-1.6.1-9.el8.x86_64.rpm                          287 kB/s |  27 kB     00:00    
(3/10): apr-util-1.6.1-9.el8.x86_64.rpm                                  349 kB/s | 106 kB     00:00    
(4/10): apr-1.6.3-12.el8.x86_64.rpm                                      405 kB/s | 129 kB     00:00    
(5/10): httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch.rpm    602 kB/s |  44 kB     00:00    
(6/10): centos-logos-httpd-85.8-2.el8.noarch.rpm                         524 kB/s |  75 kB     00:00    
(7/10): httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64.rpm         1.1 MB/s | 111 kB     00:00    
(8/10): mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64.rpm          1.0 MB/s | 155 kB     00:00    
(9/10): httpd-2.4.37-62.module_el8+657+88b2113f.x86_64.rpm               4.6 MB/s | 1.4 MB     00:00    
(10/10): mailcap-2.1.48-3.el8.noarch.rpm                                 202 kB/s |  39 kB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                    1.8 MB/s | 2.1 MB     00:01     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Installing       : apr-1.6.3-12.el8.x86_64                                                        1/10 
  Running scriptlet: apr-1.6.3-12.el8.x86_64                                                        1/10 
  Installing       : apr-util-bdb-1.6.1-9.el8.x86_64                                                2/10 
  Installing       : apr-util-openssl-1.6.1-9.el8.x86_64                                            3/10 
  Installing       : apr-util-1.6.1-9.el8.x86_64                                                    4/10 
  Running scriptlet: apr-util-1.6.1-9.el8.x86_64                                                    4/10 
  Installing       : httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                           5/10 
  Installing       : mailcap-2.1.48-3.el8.noarch                                                    6/10 
  Running scriptlet: httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                      7/10 
  Installing       : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                      7/10 
  Installing       : centos-logos-httpd-85.8-2.el8.noarch                                           8/10 
  Installing       : mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                            9/10 
  Installing       : httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                10/10 
  Running scriptlet: httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                10/10 
  Verifying        : apr-1.6.3-12.el8.x86_64                                                        1/10 
  Verifying        : apr-util-1.6.1-9.el8.x86_64                                                    2/10 
  Verifying        : apr-util-bdb-1.6.1-9.el8.x86_64                                                3/10 
  Verifying        : apr-util-openssl-1.6.1-9.el8.x86_64                                            4/10 
  Verifying        : centos-logos-httpd-85.8-2.el8.noarch                                           5/10 
  Verifying        : httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                 6/10 
  Verifying        : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                      7/10 
  Verifying        : httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                           8/10 
  Verifying        : mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                            9/10 
  Verifying        : mailcap-2.1.48-3.el8.noarch                                                   10/10 
Installed products updated.

Installed:
  apr-1.6.3-12.el8.x86_64                                                                                
  apr-util-1.6.1-9.el8.x86_64                                                                            
  apr-util-bdb-1.6.1-9.el8.x86_64                                                                        
  apr-util-openssl-1.6.1-9.el8.x86_64                                                                    
  centos-logos-httpd-85.8-2.el8.noarch                                                                   
  httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                         
  httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                              
  httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                                                   
  mailcap-2.1.48-3.el8.noarch                                                                            
  mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                                                    

Complete!
[root@stapp02 ~]# 
```


**4. Now copy the two websites folder from `Jumpserver`to app server**
`Login on Jumpserver by open new terminal`
```

thor@jump_host ~$ scp -r /home/thor/official steve@stapp02:/tmp
steve@stapp02's password: 
index.html                                                                                                                                 100%  121   142.3KB/s   00:00    
thor@jump_host ~$ scp -r /home/thor/apps steve@stapp02:/tmp
steve@stapp02's password: 
index.html                                                                                                                                       100%  117   265.0KB/s   00:00    
thor@jump_host ~$ 
```

**5. Edit configuration file `vi etc/httpd/conf/httpd.conf` & change Listen port to  `8087`**

```
[root@stapp02 ~]# vi /etc/httpd/conf/httpd.conf
```
- change the  Listen 80-> Listen 8087
```
User apache
Group apache

# 'Main' server configuration
#

#
# Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, instead of the default. See also the <VirtualHost>
# directive.
#
# Change this to Listen on specific IP addresses as shown below to
# prevent Apache from glomming onto all bound IP addresses.
#
#Listen 12.34.56.78:80
Listen 8087

#
# Dynamic Shared Object (DSO) Support
#
# To be able to use the functionality of a module which was built as a DSO you
# have to place corresponding `LoadModule' lines at this location so the
# directives contained in it are actually available _before_ they are used.
# Statically compiled modules (those listed by `httpd -l') do not need
# to be loaded here.
#
# Example:
# LoadModule foo_module modules/mod_foo.so
#
Include conf.modules.d/*.conf

#
# If you wish httpd to run as a different user or group, you must run
# httpd as root initially and it will switch.
#
# User/Group: The name (or #number) of the user/group to run httpd as.
# It is usually good practice to create a dedicated user and group for
# running httpd, as with most system services.
#
User apache
Group apache
```
- Back to terminal
```
[root@stapp02 ~]# cat /etc/httpd/conf/httpd.conf |grep Listen
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on specific IP addresses as shown below to 
#Listen 12.34.56.78:80
Listen 8087
[root@stapp02 conf]# 
```

**6. Start & enable the httpd service below commands**
```
[root@stapp02 ~]# systemctl restart httpd
[root@stapp02 ~]# systemctl enable httpd && systemctl start httpd && systemctl status  httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2023-11-04 12:59:39 UTC; 19s ago
     Docs: man:httpd.service(8)
 Main PID: 2610 (httpd)
   Status: "Running, listening on: port 8087"
    Tasks: 212 (limit: 1340692)
   Memory: 20.0M
   CGroup: /docker/1a8775448e144cd07ef5b38b37b9e623f0ee1cd5289e42ffae7721abbf0a313f/system.slice/httpd.service
           ├─2610 /usr/sbin/httpd -DFOREGROUND
           ├─2623 /usr/sbin/httpd -DFOREGROUND
           ├─2624 /usr/sbin/httpd -DFOREGROUND
           ├─2625 /usr/sbin/httpd -DFOREGROUND
           └─2626 /usr/sbin/httpd -DFOREGROUND

Nov 04 12:59:39 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 2610 (READY=1, STATUS=Started, listening on: p
ort 8087, MAINPID=2610)
Nov 04 12:59:48 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 2610 (READY=1, STATUS=Running, listening on: p
ort 8087)
Nov 04 12:59:58 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Changed dead -> running
Nov 04 12:59:58 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Failed to set invocation ID on control group /docker/1a8775448e144cd07ef5b38b37b
9e623f0ee1cd5289e42ffae7721abbf0a313f/system.slice/httpd.service, ignoring: Operation not permitted
Nov 04 12:59:58 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 2610 (READY=1, STATUS=Running, listening on: p
ort 8087)
Nov 04 12:59:58 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Failed to send unit change signal for httpd.service: Connection reset by peer
Nov 04 12:59:58 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Trying to enqueue job httpd.service/start/replace
Nov 04 12:59:58 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Installed new job httpd.service/start as 317
Nov 04 12:59:58 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Enqueued job httpd.service/start as 317
Nov 04 12:59:58 stapp02.stratos.xfusioncorp.com systemd[1]: httpd.service: Job httpd.service/start finished, result=done
[root@stapp02 ~]#
```
**7.Move copied folders from `tmp` to `/var/www/html` folder**

```

[root@stapp02 ~]# mv /tmp/official/  /var/www/html/
[root@stapp02 ~]# mv /tmp/apps/  /var/www/html/

[root@stapp02 ~]# ls /var/www/html/
apps  official
[root@stapp02 ~]# 

```
**8. Validate the task by below command from Jumpserver  `curl http://localhost:8087/official/`**

```
[root@stapp02 ~]# curl http://localhost:8087/official/
<!DOCTYPE html>
<html>
<body>

<h1>KodeKloud</h1>

<p>This is a sample page for our official website</p>

</body>
</html>

```
**9. Validate the task by below command from Jumpserver  `curl http://localhost:8087/apps/`**
```
[root@stapp02 ~]# curl http://localhost:8087/apps/
<!DOCTYPE html>
<html>
<body>

<h1>KodeKloud</h1>

<p>This is a sample page for our apps website</p>

</body>
</html>
[root@stapp02 ~]# 
```

**10. Click on `Finish` & `Confirm` to complete the task successful**