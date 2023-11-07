

## Question

The Nautilus development team had a meeting with the DevOps team where they discussed automating the deployment of one of their apps using Jenkins (the one in  `Stratos Datacenter`). They want to auto deploy the new changes in case any developer pushes to the repository. As per the requirements mentioned below configure the required Jenkins job.  
  

  

Click on the  `Jenkins`  button on the top bar to access the Jenkins UI. Login using username  `admin`  and  `Adm!n321`  password.  
  

Similarly, you can access the  `Gitea UI`  using  `Gitea`  button, username and password for Git is  `sarah`  and  `Sarah_pass123`  respectively. Under user  `sarah`  you will find a repository named  `web`  that is already cloned on the  `Storage server`  under sarah's home.  `sarah`  is a developer who is working on this repository.  
  

1. Install  `httpd`  (whatever version is available in the yum repo by default) and configure it to serve on port  `8080`  on All app servers. You can make it part of your Jenkins job or you can do this step manually on all app servers.  
  

2. Create a Jenkins job named  `nautilus-app-deployment`  and configure it in a way so that if anyone pushes any new change to the origin repository in  `master`  branch, the job should auto build and deploy the latest code on the  `Storage server`  under  `/var/www/html`  directory. Since  `/var/www/html`  on  `Storage server`  is shared among all apps.  
  

3. SSH into  `Storage Server`  using  `sarah`  user credentials mentioned above. Under sarah user's home you will find a cloned Git repository named  `web`. Under this repository there is an  `index.html`  file, update its content to  `Welcome to the xFusionCorp Industries`, then push the changes to the  `origin`  into  `master`  branch. This push must trigger your Jenkins job and the latest changes must be deployed on the servers, also make sure it deploys the entire repository content not only  `index.html`  file.  
  

Click on the  `App`  button on the top bar to access the app, you should be able to see the latest changes you deployed. Please make sure the required content is loading on the main URL  `https://<LBR-URL>`  i.e there should not be any sub-directory like  `https://<LBR-URL>/web`  etc.  
  

`Note:`  
1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on  `Restart Jenkins when installation is complete and no jobs are running`  on plugin installation/update page i.e  `update centre`. Also some times Jenkins UI gets stuck when Jenkins service restarts in the back end so in such case please make sure to refresh the UI page.  
  

2. Make sure Jenkins job passes even on repetitive runs as validation may try to build the job multiple times.  
  

3. Deployment related tasks should be done by  `sudo`  user on the destination server to avoid any permission issues so make sure to configure your Jenkins job accordingly.  
  

4. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.



## Solution

**1. Install  `httpd`  (whatever version is available in the yum repo by default) and configure it to serve on port  `8080`  on All app servers.**

- App server1 - tony
```

thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:Q17Qr6pHrT3wxnSSs1SMvb/D2Puwh78Buks6mc8fFF4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01,172.16.238.10' (ECDSA) to the list of known hosts.
tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: Ir0nM@n
[root@stapp01 ~]# 

[root@stapp01 ~]#  yum install httpd -y
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                                                                                                         14 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                                                                                                         37 MB/s |  34 MB     00:00    
CentOS Stream 8 - BaseOS                                                                                                                            25 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                                                                                            46 MB/s |  52 MB     00:01    
CentOS Stream 8 - Extras                                                                                                                            23 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                                                                                            81  B/s | 3.0 kB     00:38    
CentOS Stream 8 - Extras common packages                                                                                                           114  B/s | 6.9 kB     01:02    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                                                                                                     1.6 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                                                                                                  7.8 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                                                                                          326 kB/s | 100 kB     00:00    
Dependencies resolved.
===================================================================================================================================================================================
 Package                                     Architecture                    Version                                                      Repository                          Size
===================================================================================================================================================================================
Installing:
 httpd                                       x86_64                          2.4.37-62.module_el8+657+88b2113f                            appstream                          1.4 M
Installing dependencies:
 apr                                         x86_64                          1.6.3-12.el8                                                 appstream                          129 k
 apr-util                                    x86_64                          1.6.1-9.el8                                                  appstream                          106 k
 centos-logos-httpd                          noarch                          85.8-2.el8                                                   appstream                           75 k
 httpd-filesystem                            noarch                          2.4.37-62.module_el8+657+88b2113f                            appstream                           44 k
 httpd-tools                                 x86_64                          2.4.37-62.module_el8+657+88b2113f                            appstream                          111 k
 mailcap                                     noarch                          2.1.48-3.el8                                                 baseos                              39 k
 mod_http2                                   x86_64                          1.15.7-8.module_el8+452+6213a5e6.3                           appstream                          155 k
Installing weak dependencies:
 apr-util-bdb                                x86_64                          1.6.1-9.el8                                                  appstream                           25 k
 apr-util-openssl                            x86_64                          1.6.1-9.el8                                                  appstream                           27 k
Enabling module streams:
 httpd                                                                       2.4                                                                                                  

Transaction Summary
===================================================================================================================================================================================
Install  10 Packages

Total download size: 2.1 M
Installed size: 5.6 M
Downloading Packages:
(1/10): apr-util-bdb-1.6.1-9.el8.x86_64.rpm                                                                                                        213 kB/s |  25 kB     00:00    
(2/10): apr-util-1.6.1-9.el8.x86_64.rpm                                                                                                            711 kB/s | 106 kB     00:00    
(3/10): apr-1.6.3-12.el8.x86_64.rpm                                                                                                                824 kB/s | 129 kB     00:00    
(4/10): apr-util-openssl-1.6.1-9.el8.x86_64.rpm                                                                                                    636 kB/s |  27 kB     00:00    
(5/10): centos-logos-httpd-85.8-2.el8.noarch.rpm                                                                                                   1.2 MB/s |  75 kB     00:00    
(6/10): httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch.rpm                                                                              875 kB/s |  44 kB     00:00    
(7/10): httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64.rpm                                                                                   2.3 MB/s | 111 kB     00:00    
(8/10): mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64.rpm                                                                                    1.9 MB/s | 155 kB     00:00    
(9/10): httpd-2.4.37-62.module_el8+657+88b2113f.x86_64.rpm                                                                                         9.1 MB/s | 1.4 MB     00:00    
(10/10): mailcap-2.1.48-3.el8.noarch.rpm                                                                                                           305 kB/s |  39 kB     00:00    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                              3.6 MB/s | 2.1 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                           1/1 
  Installing       : apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Running scriptlet: apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Installing       : apr-util-bdb-1.6.1-9.el8.x86_64                                                                                                                          2/10 
  Installing       : apr-util-openssl-1.6.1-9.el8.x86_64                                                                                                                      3/10 
  Installing       : apr-util-1.6.1-9.el8.x86_64                                                                                                                              4/10 
  Running scriptlet: apr-util-1.6.1-9.el8.x86_64                                                                                                                              4/10 
  Installing       : httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                     5/10 
  Installing       : mailcap-2.1.48-3.el8.noarch                                                                                                                              6/10 
  Running scriptlet: httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Installing       : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Installing       : centos-logos-httpd-85.8-2.el8.noarch                                                                                                                     8/10 
  Installing       : mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                                                                                                      9/10 
  Installing       : httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                          10/10 
  Running scriptlet: httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                          10/10 
  Verifying        : apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Verifying        : apr-util-1.6.1-9.el8.x86_64                                                                                                                              2/10 
  Verifying        : apr-util-bdb-1.6.1-9.el8.x86_64                                                                                                                          3/10 
  Verifying        : apr-util-openssl-1.6.1-9.el8.x86_64                                                                                                                      4/10 
  Verifying        : centos-logos-httpd-85.8-2.el8.noarch                                                                                                                     5/10 
  Verifying        : httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                           6/10 
  Verifying        : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Verifying        : httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                     8/10 
  Verifying        : mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                                                                                                      9/10 
  Verifying        : mailcap-2.1.48-3.el8.noarch                                                                                                                             10/10 
Installed products updated.

Installed:
  apr-1.6.3-12.el8.x86_64                                         apr-util-1.6.1-9.el8.x86_64                                apr-util-bdb-1.6.1-9.el8.x86_64                     
  apr-util-openssl-1.6.1-9.el8.x86_64                             centos-logos-httpd-85.8-2.el8.noarch                       httpd-2.4.37-62.module_el8+657+88b2113f.x86_64      
  httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch       httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64       mailcap-2.1.48-3.el8.noarch                         
  mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64            

Complete!
```
- Before 8080 port

![1 app](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4aa4e21f-206f-4dea-a910-b02930e79b27)

```
[root@stapp01 ~]# sudo sed -i 's/Listen 80/Listen 8080/g' /etc/httpd/conf/httpd.conf
[root@stapp01 ~]# sudo systemctl restart httpd
[root@stapp01 ~]# 
```
- After 8080 port

![2 welcome](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9ae1823b-8bcd-413d-9cd0-d2931591e47e)


 - App server2 - Steve
```

Last login: Sat Nov  4 04:04:57 UTC 2023 on pts/0
thor@jump_host ~$ ssh steve@stapp02
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:84QZV7gTr8fTpa2xroeF9tJur8cqVn6Il4Jsxccfcl4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02,172.16.238.11' (ECDSA) to the list of known hosts.
steve@stapp02's password: 
[steve@stapp02 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[root@stapp02 ~]# 

[root@stapp02 ~]#  yum install httpd -y
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                                                                                                         29 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                                                                                                         35 MB/s |  34 MB     00:00    
CentOS Stream 8 - BaseOS                                                                                                                            10 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                                                                                            30 MB/s |  52 MB     00:01    
CentOS Stream 8 - Extras                                                                                                                            24 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                                                                                            99  B/s | 3.0 kB     00:31    
CentOS Stream 8 - Extras common packages                                                                                                           114  B/s | 6.9 kB     01:02    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                                                                                                     2.4 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                                                                                                  7.1 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                                                                                          363 kB/s | 100 kB     00:00    
Dependencies resolved.
===================================================================================================================================================================================
 Package                                     Architecture                    Version                                                      Repository                          Size
===================================================================================================================================================================================
Installing:
 httpd                                       x86_64                          2.4.37-62.module_el8+657+88b2113f                            appstream                          1.4 M
Installing dependencies:
 apr                                         x86_64                          1.6.3-12.el8                                                 appstream                          129 k
 apr-util                                    x86_64                          1.6.1-9.el8                                                  appstream                          106 k
 centos-logos-httpd                          noarch                          85.8-2.el8                                                   appstream                           75 k
 httpd-filesystem                            noarch                          2.4.37-62.module_el8+657+88b2113f                            appstream                           44 k
 httpd-tools                                 x86_64                          2.4.37-62.module_el8+657+88b2113f                            appstream                          111 k
 mailcap                                     noarch                          2.1.48-3.el8                                                 baseos                              39 k
 mod_http2                                   x86_64                          1.15.7-8.module_el8+452+6213a5e6.3                           appstream                          155 k
Installing weak dependencies:
 apr-util-bdb                                x86_64                          1.6.1-9.el8                                                  appstream                           25 k
 apr-util-openssl                            x86_64                          1.6.1-9.el8                                                  appstream                           27 k
Enabling module streams:
 httpd                                                                       2.4                                                                                                  

Transaction Summary
===================================================================================================================================================================================
Install  10 Packages

Total download size: 2.1 M
Installed size: 5.6 M
Downloading Packages:
(1/10): apr-util-bdb-1.6.1-9.el8.x86_64.rpm                                                                                                        296 kB/s |  25 kB     00:00    
(2/10): apr-util-openssl-1.6.1-9.el8.x86_64.rpm                                                                                                    622 kB/s |  27 kB     00:00    
(3/10): apr-util-1.6.1-9.el8.x86_64.rpm                                                                                                            773 kB/s | 106 kB     00:00    
(4/10): apr-1.6.3-12.el8.x86_64.rpm                                                                                                                896 kB/s | 129 kB     00:00    
(5/10): httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch.rpm                                                                              1.3 MB/s |  44 kB     00:00    
(6/10): centos-logos-httpd-85.8-2.el8.noarch.rpm                                                                                                   1.1 MB/s |  75 kB     00:00    
(7/10): httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64.rpm                                                                                   2.0 MB/s | 111 kB     00:00    
(8/10): mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64.rpm                                                                                    2.5 MB/s | 155 kB     00:00    
(9/10): httpd-2.4.37-62.module_el8+657+88b2113f.x86_64.rpm                                                                                         7.2 MB/s | 1.4 MB     00:00    
(10/10): mailcap-2.1.48-3.el8.noarch.rpm                                                                                                           371 kB/s |  39 kB     00:00    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                              4.6 MB/s | 2.1 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                           1/1 
  Installing       : apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Running scriptlet: apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Installing       : apr-util-bdb-1.6.1-9.el8.x86_64                                                                                                                          2/10 
  Installing       : apr-util-openssl-1.6.1-9.el8.x86_64                                                                                                                      3/10 
  Installing       : apr-util-1.6.1-9.el8.x86_64                                                                                                                              4/10 
  Running scriptlet: apr-util-1.6.1-9.el8.x86_64                                                                                                                              4/10 
  Installing       : httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                     5/10 
  Installing       : mailcap-2.1.48-3.el8.noarch                                                                                                                              6/10 
  Running scriptlet: httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Installing       : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Installing       : centos-logos-httpd-85.8-2.el8.noarch                                                                                                                     8/10 
  Installing       : mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                                                                                                      9/10 
  Installing       : httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                          10/10 
  Running scriptlet: httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                          10/10 
  Verifying        : apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Verifying        : apr-util-1.6.1-9.el8.x86_64                                                                                                                              2/10 
  Verifying        : apr-util-bdb-1.6.1-9.el8.x86_64                                                                                                                          3/10 
  Verifying        : apr-util-openssl-1.6.1-9.el8.x86_64                                                                                                                      4/10 
  Verifying        : centos-logos-httpd-85.8-2.el8.noarch                                                                                                                     5/10 
  Verifying        : httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                           6/10 
  Verifying        : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Verifying        : httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                     8/10 
  Verifying        : mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                                                                                                      9/10 
  Verifying        : mailcap-2.1.48-3.el8.noarch                                                                                                                             10/10 
Installed products updated.

Installed:
  apr-1.6.3-12.el8.x86_64                                         apr-util-1.6.1-9.el8.x86_64                                apr-util-bdb-1.6.1-9.el8.x86_64                     
  apr-util-openssl-1.6.1-9.el8.x86_64                             centos-logos-httpd-85.8-2.el8.noarch                       httpd-2.4.37-62.module_el8+657+88b2113f.x86_64      
  httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch       httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64       mailcap-2.1.48-3.el8.noarch                         
  mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64            

Complete!
```
- Before 8080 port

![1 app](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4aa4e21f-206f-4dea-a910-b02930e79b27)

```
[root@stapp02 ~]# 
[root@stapp02 ~]# sudo sed -i 's/Listen 80/Listen 8080/g' /etc/httpd/conf/httpd.conf
[root@stapp02 ~]# sudo systemctl restart httpd
[root@stapp02 ~]# 
```
- After 8080 port

![2 welcome](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9ae1823b-8bcd-413d-9cd0-d2931591e47e)


 - App server3 - banner
```
banner
Last login: Sat Nov  4 04:05:27 UTC 2023 on pts/2
thor@jump_host ~$ ssh banner@stapp03
The authenticity of host 'stapp03 (172.16.238.12)' can't be established.
ECDSA key fingerprint is SHA256:0Mx5z5H9VUzI8BZil6zHmBKDGLIJ5/O4/FT7GZrAUL4.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.
banner@stapp03's password: 
[banner@stapp03 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[root@stapp03 ~]# 

[root@stapp03 ~]#  yum install httpd -y
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                                                                                                         15 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                                                                                                         37 MB/s |  34 MB     00:00    
CentOS Stream 8 - BaseOS                                                                                                                           9.5 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                                                                                            52 MB/s |  52 MB     00:00    
CentOS Stream 8 - Extras                                                                                                                           9.4 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                                                                                            98  B/s | 3.0 kB     00:31    
CentOS Stream 8 - Extras common packages                                                                                                           114  B/s | 6.9 kB     01:02    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                                                                                                     1.8 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                                                                                                  7.9 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                                                                                          423 kB/s | 100 kB     00:00    
Dependencies resolved.
===================================================================================================================================================================================
 Package                                     Architecture                    Version                                                      Repository                          Size
===================================================================================================================================================================================
Installing:
 httpd                                       x86_64                          2.4.37-62.module_el8+657+88b2113f                            appstream                          1.4 M
Installing dependencies:
 apr                                         x86_64                          1.6.3-12.el8                                                 appstream                          129 k
 apr-util                                    x86_64                          1.6.1-9.el8                                                  appstream                          106 k
 centos-logos-httpd                          noarch                          85.8-2.el8                                                   appstream                           75 k
 httpd-filesystem                            noarch                          2.4.37-62.module_el8+657+88b2113f                            appstream                           44 k
 httpd-tools                                 x86_64                          2.4.37-62.module_el8+657+88b2113f                            appstream                          111 k
 mailcap                                     noarch                          2.1.48-3.el8                                                 baseos                              39 k
 mod_http2                                   x86_64                          1.15.7-8.module_el8+452+6213a5e6.3                           appstream                          155 k
Installing weak dependencies:
 apr-util-bdb                                x86_64                          1.6.1-9.el8                                                  appstream                           25 k
 apr-util-openssl                            x86_64                          1.6.1-9.el8                                                  appstream                           27 k
Enabling module streams:
 httpd                                                                       2.4                                                                                                  

Transaction Summary
===================================================================================================================================================================================
Install  10 Packages

Total download size: 2.1 M
Installed size: 5.6 M
Downloading Packages:
(1/10): apr-util-bdb-1.6.1-9.el8.x86_64.rpm                                                                                                        286 kB/s |  25 kB     00:00    
(2/10): apr-util-1.6.1-9.el8.x86_64.rpm                                                                                                            764 kB/s | 106 kB     00:00    
(3/10): apr-util-openssl-1.6.1-9.el8.x86_64.rpm                                                                                                    509 kB/s |  27 kB     00:00    
(4/10): apr-1.6.3-12.el8.x86_64.rpm                                                                                                                890 kB/s | 129 kB     00:00    
(5/10): centos-logos-httpd-85.8-2.el8.noarch.rpm                                                                                                   1.9 MB/s |  75 kB     00:00    
(6/10): httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch.rpm                                                                              1.2 MB/s |  44 kB     00:00    
(7/10): httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64.rpm                                                                                   2.6 MB/s | 111 kB     00:00    
(8/10): mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64.rpm                                                                                    2.8 MB/s | 155 kB     00:00    
(9/10): httpd-2.4.37-62.module_el8+657+88b2113f.x86_64.rpm                                                                                         8.2 MB/s | 1.4 MB     00:00    
(10/10): mailcap-2.1.48-3.el8.noarch.rpm                                                                                                           312 kB/s |  39 kB     00:00    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                              2.7 MB/s | 2.1 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                           1/1 
  Installing       : apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Running scriptlet: apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Installing       : apr-util-bdb-1.6.1-9.el8.x86_64                                                                                                                          2/10 
  Installing       : apr-util-openssl-1.6.1-9.el8.x86_64                                                                                                                      3/10 
  Installing       : apr-util-1.6.1-9.el8.x86_64                                                                                                                              4/10 
  Running scriptlet: apr-util-1.6.1-9.el8.x86_64                                                                                                                              4/10 
  Installing       : httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                     5/10 
  Installing       : mailcap-2.1.48-3.el8.noarch                                                                                                                              6/10 
  Running scriptlet: httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Installing       : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Installing       : centos-logos-httpd-85.8-2.el8.noarch                                                                                                                     8/10 
  Installing       : mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                                                                                                      9/10 
  Installing       : httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                          10/10 
  Running scriptlet: httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                          10/10 
  Verifying        : apr-1.6.3-12.el8.x86_64                                                                                                                                  1/10 
  Verifying        : apr-util-1.6.1-9.el8.x86_64                                                                                                                              2/10 
  Verifying        : apr-util-bdb-1.6.1-9.el8.x86_64                                                                                                                          3/10 
  Verifying        : apr-util-openssl-1.6.1-9.el8.x86_64                                                                                                                      4/10 
  Verifying        : centos-logos-httpd-85.8-2.el8.noarch                                                                                                                     5/10 
  Verifying        : httpd-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                           6/10 
  Verifying        : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                                                                                7/10 
  Verifying        : httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64                                                                                                     8/10 
  Verifying        : mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64                                                                                                      9/10 
  Verifying        : mailcap-2.1.48-3.el8.noarch                                                                                                                             10/10 
Installed products updated.

Installed:
  apr-1.6.3-12.el8.x86_64                                         apr-util-1.6.1-9.el8.x86_64                                apr-util-bdb-1.6.1-9.el8.x86_64                     
  apr-util-openssl-1.6.1-9.el8.x86_64                             centos-logos-httpd-85.8-2.el8.noarch                       httpd-2.4.37-62.module_el8+657+88b2113f.x86_64      
  httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch       httpd-tools-2.4.37-62.module_el8+657+88b2113f.x86_64       mailcap-2.1.48-3.el8.noarch                         
  mod_http2-1.15.7-8.module_el8+452+6213a5e6.3.x86_64            

Complete!
```
- Before 8080 port

![1 app](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4aa4e21f-206f-4dea-a910-b02930e79b27)

```
[root@stapp03 ~]# sudo sed -i 's/Listen 80/Listen 8080/g' /etc/httpd/conf/httpd.conf
[root@stapp03 ~]# sudo systemctl restart httpd
[root@stapp03 ~]# 
```
- After 8080 port

![2 welcome](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9ae1823b-8bcd-413d-9cd0-d2931591e47e)


**2. Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8081 and click on Display Port. You should be able to access the Jenkins login page. Login using username the  `admin`  and  `Adm!n321`  password.**

![3](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/a381918e-7f79-46d6-a562-5ad5fa35a5ed)

**3. Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.**
![3](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/96f47695-1f74-4f44-8fb5-ac1428bc77c5)

**4. Search for ssh , ssh credentials, ssh build agents & git -> select all plugins checkbox click and click Download now and install after restart.**

![4](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/b2721001-c294-41a8-8e4a-0eba6544380a)


-   click Download now and install after restart.

![5](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/411a9c59-9d1c-4bf7-bda8-1aabd46b19c1)

![5 1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e2dcfd1c-3741-4d46-8400-796e68323634)

-   Restart jenkins

**5. Click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`.**

![6](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/b792b7eb-9fd6-4d06-a354-a896287ee559)


-   Click the repository named  `sarah/web-app`  in Gitea. Inside Docker images. -  `copy the Git link.`

![7](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3f196a59-8d3e-45cf-81fd-f084aaff718a)


**6. Create a Jenkins job named  `nautilus-app-deployment`**


![8](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3df7dc6c-cb44-4078-8432-770ef3ca8ffe)

- Click ok

- Choose git option  `paste the git link`from Gitea.

```
-git
Repo URL : http://git.stratos.xfusioncorp.com/sarah/web.git
credentials : none
Branches to build
Branches specifier : */master

-Build trigger
poll scm 
schedule : * * * * * (every min)
```


![9](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d74019d2-ff38-4376-bfd1-e758a764879f)


![10](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/b825d6ac-4257-430d-83d7-2b8706d13fed)

- Click save.

- Build Automatically

![11](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/6499d7c8-8800-4efe-beb4-1421abe79f8a)

- Click #1

![12](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/5ca4fa75-386e-49c3-981c-edb4a5919b24)

- Build success #1

![13](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/6b838181-a366-4e74-9278-514ee5e9aa0e)

- Console output
```
# Console Output

Started by an SCM change
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/nautilus-app-deployment
The recommended git tool is: NONE
No credentials specified
Cloning the remote Git repository
Cloning repository [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git)
 > git init /var/lib/jenkins/workspace/nautilus-app-deployment # timeout=10
Fetching upstream changes from [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git)
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
 > git fetch --tags --force --progress -- [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision 64b9de182e58195fc58cdafc3936d086d0586736 (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 64b9de182e58195fc58cdafc3936d086d0586736 # timeout=10
Commit message: "Added index.html file"
First time build. Skipping changelog.
Finished: SUCCESS

```

![14](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/7ab22e05-3554-4152-807d-57f55d440a97)


- Back to terminal session

- Login `Jenkins server`

```

thor@jump_host ~$ ssh jenkins@jenkins
jenkins@jenkins's password: 
Permission denied, please try again.
jenkins@jenkins's password: j@vis
Welcome to Ubuntu 20.04.5 LTS (GNU/Linux 5.4.0-1106-gcp x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
```
- Generate `ssh-key`
```

jenkins@jenkins:~$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/var/lib/jenkins/.ssh/id_rsa): 
Created directory '/var/lib/jenkins/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /var/lib/jenkins/.ssh/id_rsa
Your public key has been saved in /var/lib/jenkins/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:sWk17NXQBcYLUOM+2Nw5/dFTDoG48raok0F1IbEtYIA jenkins@jenkins.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 3072]----+
|   ...o o.o=+++o.|
|  E  . ..=o.o=o. |
|       .+.=.o.o..|
|      . .B.B ..=o|
|     .  Soo = +o+|
|      ..  o  . .+|
|       o o .    .|
|      o . .      |
|      .o         |
+----[SHA256]-----+
jenkins@jenkins:~$ 

jenkins@jenkins:~$ cd .ssh/
jenkins@jenkins:~/.ssh$ ls
id_rsa  id_rsa.pub
jenkins@jenkins:~/.ssh$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDGzMPYg/Ypk5sKPJCE0mDkagg0jWEw5LxDjLhGIN7AunDI25wD2TRHGiDUF1/s77Y2GuaMKCxbbVNADL0ZXUtpBtDm9Y4LugVJGo43LueTV5c2cXNxU7it6DWam7L1M0zFMZxnidkUurt5yXsF6DI3scI2kNzsqwJsOECBVETeEX5VIzpgUUN1siNaCF8choUkxb3b7WxVjYhPY6NWln9nTTjxH+YyYcipofuPhtTKRIz+DTir9tnV5u//KRs8F2u0eHhFEpFJCBhH9SYxgMfsS49dcUUyGVSoZCSE5WkHIfT187+FDeDNRRVQRGvthd4V64U1sBHv/76ow3q4qy+aLBeVTIYfJ7bylioY3NkDkGftXSkl2p10SI3zn1RNmHJq1TVwvsofYG91hER540O7VXg0O6xS/yF5P3ymae0TZeKXuuv5oSGb//F8EseYUaLnjJefEJJJx3Uqnac3JZ0BMme9wYAUpQv0unvT4mRHCWNWdYSRsA9dnwWDS/nRsnE= jenkins@jenkins.stratos.xfusioncorp.com
jenkins@jenkins:~/.ssh$  
```

- Storage server `ssh` and `Authorized key`
```
thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:Hb97jbZzyRqK4y3Ux0N4HNEqXz8kvwW8mrSB0icFIt8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
natasha@ststor01's password: Bl@kW
[natasha@ststor01 ~]$ sudo su
[sudo] password for natasha: Bl@kW
[root@ststor01 natasha]# pwd
/home/natasha
[root@ststor01 natasha]# ls -a
.  ..  .bash_logout  .bash_profile  .bashrc
[root@ststor01 natasha]#

[root@ststor01 natasha]# mkdir .ssh
[root@ststor01 natasha]# cat > .ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDGzMPYg/Ypk5sKPJCE0mDkagg0jWEw5LxDjLhGIN7AunDI25wD2TRHGiDUF1/s77Y2GuaMKCxbbVNADL0ZXUtpBtDm9Y4LugVJGo43LueTV5c2cXNxU7it6DWam7L1M0zFMZxnidkUurt5yXsF6DI3scI2kNzsqwJsOECBVETeEX5VIzpgUUN1siNaCF8choUkxb3b7WxVjYhPY6NWln9nTTjxH+YyYcipofuPhtTKRIz+DTir9tnV5u//KRs8F2u0eHhFEpFJCBhH9SYxgMfsS49dcUUyGVSoZCSE5WkHIfT187+FDeDNRRVQRGvthd4V64U1sBHv/76ow3q4qy+aLBeVTIYfJ7bylioY3NkDkGftXSkl2p10SI3zn1RNmHJq1TVwvsofYG91hER540O7VXg0O6xS/yF5P3ymae0TZeKXuuv5oSGb//F8EseYUaLnjJefEJJJx3Uqnac3JZ0BMme9wYAUpQv0unvT4mRHCWNWdYSRsA9dnwWDS/nRsnE= jenkins@jenkins.stratos.xfusioncorp.com
````
- jenkins server connect with `natasha` without password 

```
jenkins@jenkins:~/.ssh$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:GGG/LO7KrSuvOIs6aN59g/d1zCQyF/NXzi7KCxS9+yI.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
Last login: Sat Nov  4 05:29:48 2023 from 172.16.238.2
[natasha@ststor01 ~]$ 
```
 - Go to jenkins and configure `scp` command

```
Build step: command 
scp * natasha@ststor1:/var/www/html
```
![15](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4e712ddd-933b-4684-b3f3-62d9c739fd23)

- click save.

- After build now get error #2 failed

![16](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/69bade94-8878-4c15-b603-bc19153a5779)

 - Check console output

```
# Console Output

Started by user [admin](https://8080-port-2a0e6625feca476e.labs.kodekloud.com/user/admin)
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/nautilus-app-deployment
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/nautilus-app-deployment/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) # timeout=10
Fetching upstream changes from [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git)
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
 > git fetch --tags --force --progress -- [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision bdb0df33b9bfae2295a0affe52a2f91dd5f96f6d (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f bdb0df33b9bfae2295a0affe52a2f91dd5f96f6d # timeout=10
Commit message: "Added index.html file"
 > git rev-list --no-walk bdb0df33b9bfae2295a0affe52a2f91dd5f96f6d # timeout=10
[nautilus-app-deployment] $ /bin/sh -xe /tmp/jenkins14516956074333607045.sh
+ scp index.html natasha@ststor01:/var/www/html
scp: /var/www/html/index.html: Permission denied
Build step 'Execute shell' marked build as failure
Finished: FAILURE
```
![17](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/0c2d8be6-8dd0-4f51-aed2-2b2ea75745c5)



- Back to storage server
-  change root directory to natasha.

```
[root@ststor01 natasha]# cd /var/www/html
[root@ststor01 html]# ls
index.html
[root@ststor01 html]# ls -l
total 4
-rw-r--r-- 1 sarah sarah 8 Nov  4 05:14 index.html
[root@ststor01 html]# cd ..
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 root root 4096 Aug 17 14:09 html
[root@ststor01 www]# chown natasha -R html/
[root@ststor01 www]# ls
html
[root@ststor01 www]# ls -l
total 4
drwxr-xr-x 3 natasha root 4096 Aug 17 14:09 html
[root@ststor01 www]# 
[root@ststor01 www]# ls
html
[root@ststor01 www]# cd html
[root@ststor01 html]# exit
exit
[natasha@ststor01 ~]$ exit
logout
Connection to ststor01 closed.
thor@jump_host ~$ 

```
- After build now

![18](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3c71bbec-4374-4686-b2db-ab13dbfc91bf)

- Console output


```
# Console Output

Started by user [admin](https://8080-port-2a0e6625feca476e.labs.kodekloud.com/user/admin)
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/nautilus-app-deployment
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/nautilus-app-deployment/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) # timeout=10
Fetching upstream changes from [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git)
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
 > git fetch --tags --force --progress -- [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision bdb0df33b9bfae2295a0affe52a2f91dd5f96f6d (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f bdb0df33b9bfae2295a0affe52a2f91dd5f96f6d # timeout=10
Commit message: "Added index.html file"
 > git rev-list --no-walk bdb0df33b9bfae2295a0affe52a2f91dd5f96f6d # timeout=10
[nautilus-app-deployment] $ /bin/sh -xe /tmp/jenkins10636569455146193267.sh
+ scp index.html natasha@ststor01:/var/www/html
Finished: SUCCESS
```
- Build success. #3


![19](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/b336d83c-6e94-4631-88fc-2605c3c2543a)


**6. SSH into  `Storage Server`  using  `sarah`**

```
thor@jump_host ~$ ssh sarah@ststor01
sarah@ststor01's password: 
[sarah@ststor01 ~]$ ls
[sarah@ststor01 ~]$ pwd
/home/sarah
```
![20 gitea](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/aa3f9b49-098a-4845-ae7f-f42d669af910)

- Copy the git link and clone it.
```
[sarah@ststor01 ~]$ git clone http://git.stratos.xfusioncorp.com/sarah/web.git
Cloning into 'web'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
[sarah@ststor01 ~]$ ls
web
[sarah@ststor01 ~]$ cd web/
[sarah@ststor01 web]$ ls
index.html
[sarah@ststor01 web]$ cat index.html
Welcome
[sarah@ststor01 web]$ cat > index.html
Welcome to the xFusionCorp Industries
^C
[sarah@ststor01 web]$ git commit -am "Updated index.html"
[master 223655e] Updated index.html
 1 file changed, 1 insertion(+), 1 deletion(-)
[sarah@ststor01 web]$ git push origin master
Username for 'http://git.stratos.xfusioncorp.com': sarah
Password for 'http://sarah@git.stratos.xfusioncorp.com': Sarah_pass123
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 290 bytes | 290.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: . Processing 1 references
remote: Processed 1 references in total
To http://git.stratos.xfusioncorp.com/sarah/web.git
   bdb0df3..223655e  master -> master
[sarah@ststor01 web]$ 
```

- After changed in Gitea Repo.

![21](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/67becc27-320f-4274-831e-70b3427eed69)

- Changed the index.html file.

![22](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/f737d035-22e4-4e5e-84c2-2dc67e4e41fd)

- Click Build Now

![23](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3ec9a0d0-5e87-45ca-9323-246adeba60d7)


- Console output

```
# Console Output

Started by an SCM change
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/nautilus-app-deployment
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/nautilus-app-deployment/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) # timeout=10
Fetching upstream changes from [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git)
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
 > git fetch --tags --force --progress -- [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision 223655e79d6b2e2604c4f5fa0cd7bfdb1409bf79 (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 223655e79d6b2e2604c4f5fa0cd7bfdb1409bf79 # timeout=10
Commit message: "Updated index.html"
 > git rev-list --no-walk bdb0df33b9bfae2295a0affe52a2f91dd5f96f6d # timeout=10
[nautilus-app-deployment] $ /bin/sh -xe /tmp/jenkins8340572058397896763.sh
+ scp index.html natasha@ststor01:/var/www/html
Finished: SUCCESS
```

- Automatically build deploy #4

![24](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/8387c96b-f5ed-4781-b25e-301362fe6088)

- Before Build

![25](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/f4517228-9e57-425a-b22e-59b4850da47f)

- After Build

![26](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4ccf4c6a-ad38-41bc-b597-76c1994e5c5a)


**7. Click on  `Finish`  &  `Confirm`  to complete the task successfully**


![28](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/dbcc2c1f-c98c-4c88-9ac4-d3e08fb8b931)

![29](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d7fe62dd-fef2-43f8-aa40-33f084dc74b6)

