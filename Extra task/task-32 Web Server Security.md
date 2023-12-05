

## Questions:
During a recent security audit, the application security team of xFusionCorp Industries found security issues with the Apache web server on Nautilus `App Server 3` server in Stratos DC. They have listed several security issues that need to be fixed on this server. Please apply the security settings below:



a. On Nautilus `App Server 3` it was identified that the Apache web server is exposing the version number. Ensure this server has the appropriate settings to hide the version number of the Apache web server.

b. There is a website hosted under `/var/www/html/beta` on `App Server 3`. It was detected that the directory /beta lists all of its contents while browsing the URL. Disable the directory browser listing in Apache config.

c. Also make sure to restart the Apache service after making the changes.


## Solution: 

**1. At first login on app server  & Switch to root user** 

```
thor@jump_host ~$ ssh banner@stapp03
banner@stapp01's password: BigGr33n

[banner@stapp03 ~]$ sudo su -
[sudo] password for banner: BigGr33n
```

**2. Run the Below command to check the existing Apache HTTPd service status**

```
[root@stapp03 ~]# systemctl status httpd
Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:httpd(8)
           man:apachectl(8)
```

**3. Confirm the existing config**

```
[root@stapp03 ~]# cat /etc/httpd/conf/httpd.conf  |grep ServerTokens
[root@stapp03 ~]# cat /etc/httpd/conf/httpd.conf  |grep ServerSignature
[root@stapp03 ~]# cat /etc/httpd/conf/httpd.conf  |grep Indexes
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
    Options Indexes FollowSymLinks
```

**5. Edit the  /etc/httpd/conf/httpd.conf  file and Added below lines end of config file & Save it Also Modified Options  +FollowSymLinks**

```
[root@stapp03 ~]# vi /etc/httpd/conf/httpd.conf
open another terminal :
IncludeOptional conf.d/*.conf   after this line 

(ServerSignature off
ServerTokens Prod)

control+c
use search /direc
then
 Options Indexes FollowSymLinks
remove indexes - Options FollowSymLinks

exit use :wq

[root@stapp03 ~]# cat /etc/httpd/conf/httpd.conf  |grep ServerTokens
ServerTokens Prod
[root@stapp03 ~]# cat /etc/httpd/conf/httpd.conf  |grep ServerSignature
ServerSignature off
[root@stapp03 ~]# cat /etc/httpd/conf/httpd.conf  |grep Indexes
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
```

**6. Post saved config file, start the httpd services**

```
[root@stapp03 ~]# systemctl start httpd
[root@stapp03 ~]# systemctl status httpd

Active: active (running) since Sun 2023-06-25 06:11:26 UTC; 10s ago
     Docs: man:httpd(8)
           man:apachectl(8)
```

**7.  Validate Apache httpd running  as per the task request**

```
[root@stapp03 ~]# curl -I http://stapp03:8080

[root@stapp03 ~]# curl -I http://stapp03:8080/beta
```

**8.  Click on `Finish` & `Confirm` to complete the task successful**