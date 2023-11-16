

## Questions:

We have a requirement where we want to password protect a directory in the Apache web server document root. We want to password protect `http://<website-url>:<apache_port>/protected` URL as per the following requirements (you can use any `website-url` for it like `localhost` since there are no such specific requirements as of now). Setup the same on `App server 1` as per below mentioned requirements:



a. We want to use basic authentication.


b. We do not want to use `htpasswd` file based authentication. Instead, we want to use `PAM` authentication, i.e `Basic Auth + PAM` so that we can authenticate with a Linux user.


c. We already have a user `yousuf` with password `TmPcZjtRQx` which you need to provide access to.


d. You can test the same using a curl command from jump host `curl http://<website-url>:<apache_port>/protected`.



## Solution:

**1. At first login on app server  ssh tony@stapp01**

```

thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:SySamszyWhhLGFiybhGBqfrr8g55wS/3e37ZpBOvICs.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01,172.16.238.10' (ECDSA) to the list of known hosts.
tony@stapp01's password: Ir0nM@n
```

**2. Switch to  root user : sudo su -**

```

[tony@stapp01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: Ir0nM@n
```

**3. Run Below command to install  PWAUTH**

```

[root@stapp01 ~]# id yousuf
uid=1001(yousuf) gid=1001(yousuf) groups=1001(yousuf)
[root@stapp01 ~]# yum --enablerepo=epel -y install mod_authnz_external pwauth 

Complete!
```

**4. Edit the  vi /etc/httpd/conf.d/authnz_external.conf  file and Added below lines end of config file & Save it**

```

[root@stapp01 ~]# vi /etc/httpd/conf.d/authnz_external.conf
[root@stapp01 ~]# cat /etc/httpd/conf.d/authnz_external.conf

DefineExternalAuth pwauth pipe /usr/bin/pwauth

#
# see also: https://github.com/bimimicah/mod-auth-external/wiki/ConfigApache24
#

#<Location "/staff">
#
#       # Require SSL connection for password protection.
#       SSLRequireSSL
#
#       AuthType Basic
#       AuthName "Staff content"
#       AuthBasicProvider external
#       AuthExternal pwauth
#       require valid-user
#
#</Location>

<Directory /var/www/html/protected>

AuthType Basic

AuthName "PAM Authentication"

AuthBasicProvider external

AuthExternal pwauth

require valid-user

</Directory>
```

**5. Run the Below command to create a protected directory & cat the index.html file**

```

[root@stapp01 ~]# mkdir -p /var/www/html/protected
[root@stapp01 ~]# cat /var/www/html/protected/index.html
This is KodeKloud Protected Directory
```

**6. Post saved config file , start and status the httpd services**

```

[root@stapp01 ~]# systemctl start httpd && systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Fri 2023-09-22 08:10:04 UTC; 6ms ago
     Docs: man:httpd(8)
           man:apachectl(8)
 Main PID: 1039 (httpd)
   Status: "Processing requests..."
   CGroup: /docker/e093da89b6f6bf4e889f5f1f0d330ab6c39030c23b10d3015b69f0f76842b66f/system.slice/httpd.service
           ├─1039 /usr/sbin/httpd -DFOREGROUND
           ├─1040 /usr/sbin/httpd -DFOREGROUND
           ├─1042 /usr/sbin/httpd -DFOREGROUND
           ├─1043 /usr/sbin/httpd -DFOREGROUND
           ├─1044 /usr/sbin/httpd -DFOREGROUND
           └─1045 /usr/sbin/httpd -DFOREGROUND

Sep 22 08:10:04 stapp01.stratos.xfusioncorp.com systemd[1]: Starting The Apache HTTP Server...
Sep 22 08:10:04 stapp01.stratos.xfusioncorp.com httpd[1039]: AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using stapp01.stratos.xfusion...is message
Sep 22 08:10:04 stapp01.stratos.xfusioncorp.com systemd[1]: Started The Apache HTTP Server.
Hint: Some lines were ellipsized, use -l to show in full.
```

**7.  Validate Apache HTTPd running  as per the task request**

```
[root@stapp01 ~]# curl -u yousuf:TmPcZjtRQx http://localhost:8080/protected/
This is KodeKloud Protected Directory
```

**8.  Click on `Finish` & `Confirm` to complete the task successfully**
