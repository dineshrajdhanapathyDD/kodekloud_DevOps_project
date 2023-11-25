

## Questions:

There is a static website running in `Stratos Datacenter`. They have already configured the app servers and code is already deployed there. To make it work properly, they need to configure `LBR` server. There are number of options for that, but team has decided to go with `HAproxy`. FYI, apache is running on port `8087` on all app servers. Complete this task as per below details.



a. Install and configure `HAproxy` on `LBR` server using `yum` only and make sure all app servers are added to `HAproxy` load balancer. `HAproxy` must serve on default `http` port (Note: Please do not remove (stats socket `/var/lib/haproxy/stats` entry from haproxy default config.).

b. Once done, you can access the website using `StaticApp` button on the top bar.


## Solution:  

**1. At first you need to log in to all the app servers and find the Listen port & need to start the httpd services.**

**2. Login on App server  ssh tony@stapp01**

```
thor@jump_host ~$ ssh tony@stapp01

tony@stapp01's password:Ir0nM@n
```

**3. Switch to  root user : sudo su -**

```
[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n

[root@stapp01 ~]# cat /etc/httpd/conf/httpd.conf |grep Listen
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on specific IP addresses as shown below to 
#Listen 12.34.56.78:80
Listen 8087
```

**4. Enable httpd & start the service**
```
[root@stapp01 ~]# systemctl enable httpd
Created symlink /etc/systemd/system/multi-user.target.wants/httpd.service → /usr/lib/systemd/system/httpd.service.

[root@stapp01 ~]# systemctl start httpd && systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2023-08-30 18:28:51 UTC; 7min ago
     Docs: man:httpd.service(8)
 Main PID: 905 (httpd)
   Status: "Running, listening on: port 8087"
    Tasks: 212 (limit: 1340692)
   Memory: 20.5M
   CGroup: /docker/96f23e76daab4d08d56388f700f1482786718bd4b8f0c9c9a8f4eab1a7b9a336/system.slice/httpd.service
           ├─905 /usr/sbin/httpd -DFOREGROUND
           ├─919 /usr/sbin/httpd -DFOREGROUND
           ├─920 /usr/sbin/httpd -DFOREGROUND
           ├─921 /usr/sbin/httpd -DFOREGROUND
           └─922 /usr/sbin/httpd -DFOREGROUND

Aug 30 18:35:20 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 905 (READY=1, STATUS=Running, listening on: port 8087)
Aug 30 18:35:30 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 905 (READY=1, STATUS=Running, listening on: port 8087)
Aug 30 18:35:40 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 905 (READY=1, STATUS=Running, listening on: port 8087)
Aug 30 18:35:50 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 905 (READY=1, STATUS=Running, listening on: port 8087)
Aug 30 18:36:00 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 905 (READY=1, STATUS=Running, listening on: port 8087)
Aug 30 18:36:10 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message from PID 905 (READY=1, STATUS=Running, listening on: port 8087)
Aug 30 18:36:14 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Trying to enqueue job httpd.service/start/replace
Aug 30 18:36:14 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Installed new job httpd.service/start as 93
Aug 30 18:36:14 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Enqueued job httpd.service/start as 93
Aug 30 18:36:14 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Job httpd.service/start finished, result=done
```

**5. Validate Apache HTTPd running  as per the task request**
```
[root@stapp01 ~]# cat /etc/httpd/conf/httpd.conf |grep Listen
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on specific IP addresses as shown below to 
#Listen 12.34.56.78:80
Listen 8087
[root@stapp01 ~]# curl localhost:8087
Welcome to xFusionCorp Industries!
```

- terminal -1

- Now Login to the the LBR Server and install the haproxy 
**1. At first login on LB server  ssh loki@stlb01**

```
thor@jump_host ~$ ssh loki@stlb01
loki@stlb01's password:Mischi3f
```

**2. Switch to  root user : sudo su -**

```
[loki@stlb01 ~]$ sudo su -
[sudo] password for loki:Mischi3f
```

**3. install the haproxy : yum -y install haproxy**

```
[root@stlb01 ~]# yum -y install haproxy
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                                                                                                                    27 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                                                                                                                    40 MB/s |  33 MB     00:00    
CentOS Stream 8 - BaseOS                                                                                                                                       22 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                                                                                                       44 MB/s |  46 MB     00:01    
CentOS Stream 8 - Extras                                                                                                                                       18 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                                                                                                       14 kB/s | 3.0 kB     00:00    
CentOS Stream 8 - Extras common packages                                                                                                                       19 kB/s | 6.8 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                                                                                                                948 kB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                                                                                                             7.8 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                                                                                                     430 kB/s |  99 kB     00:00    
Dependencies resolved.
==============================================================================================================================================================================================
 Package                                     Architecture                               Version                                           Repository                                     Size
==============================================================================================================================================================================================
Installing:
 haproxy                                     x86_64                                     1.8.27-5.el8                                      appstream                                     1.4 M

Transaction Summary
==============================================================================================================================================================================================
Install  1 Package

Total download size: 1.4 M
Installed size: 4.2 M
Downloading Packages:
haproxy-1.8.27-5.el8.x86_64.rpm                                                                                                                               5.9 MB/s | 1.4 MB     00:00    
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                         4.0 MB/s | 1.4 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                      1/1 
  Running scriptlet: haproxy-1.8.27-5.el8.x86_64                                                                                                                                          1/1 
  Installing       : haproxy-1.8.27-5.el8.x86_64                                                                                                                                          1/1 
  Running scriptlet: haproxy-1.8.27-5.el8.x86_64                                                                                                                                          1/1 
  Verifying        : haproxy-1.8.27-5.el8.x86_64                                                                                                                                          1/1 
Installed products updated.

Installed:
  haproxy-1.8.27-5.el8.x86_64                                                                                                                                                                 

Complete!
```

**4. Edit the vi /etc/haproxy/haproxy.cfg   file and correct the changes as per below**

```
[root@stlb01 ~]# cat /etc/haproxy/haproxy.cfg |grep haproxy/stats
    stats socket /var/lib/haproxy/stats

[root@stlb01 ~]# vi /etc/haproxy/haproxy.cfg
[root@stlb01 ~]# cat /etc/haproxy/haproxy.cfg
```
#---------------------------------------------------------------------
# Example configuration for a possible web application.  See the
# full configuration options online.
#
#   https://www.haproxy.org/download/1.8/doc/configuration.txt
#
#---------------------------------------------------------------------

#---------------------------------------------------------------------
# Global settings
#---------------------------------------------------------------------
global
    # to have these messages end up in /var/log/haproxy.log you will
    # need to:
    #
    # 1) configure syslog to accept network log events.  This is done
    #    by adding the '-r' option to the SYSLOGD_OPTIONS in
    #    /etc/sysconfig/syslog
    #
    # 2) configure local2 events to go to the /var/log/haproxy.log
    #   file. A line like the following can be added to
    #   /etc/sysconfig/syslog
    #
    #    local2.*                       /var/log/haproxy.log
    #
    log         127.0.0.1 local2

    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     4000
    user        haproxy
    group       haproxy
    daemon

    # turn on stats unix socket
    stats socket /var/lib/haproxy/stats

    # utilize system-wide crypto-policies
    ssl-default-bind-ciphers PROFILE=SYSTEM
    ssl-default-server-ciphers PROFILE=SYSTEM

#---------------------------------------------------------------------
# common defaults that all the 'listen' and 'backend' sections will
# use if not designated in their block
#---------------------------------------------------------------------
defaults
    mode                    http
    log                     global
    option                  httplog
    option                  dontlognull
    option http-server-close
    option forwardfor       except 127.0.0.0/8
    option                  redispatch
    retries                 3
    timeout http-request    10s
    timeout queue           1m
    timeout connect         10s
    timeout client          1m
    timeout server          1m
    timeout http-keep-alive 10s
    timeout check           10s
    maxconn                 3000

#---------------------------------------------------------------------
# main frontend which proxys to the backends
#---------------------------------------------------------------------
frontend main
    bind *:80
    acl url_static       path_beg       -i /static /images /javascript /stylesheets
    acl url_static       path_end       -i .jpg .gif .png .css .js

    use_backend static          if url_static
    default_backend             app

#---------------------------------------------------------------------
# static backend for serving up images, stylesheets and such
#---------------------------------------------------------------------
backend static
    balance     roundrobin
    server      static 127.0.0.1:4331 check

#---------------------------------------------------------------------
# round robin balancing between the various backends
#---------------------------------------------------------------------
backend app
    balance     roundrobin
    server  stapp01 172.16.238.10:8087 check
    server  stapp02 172.16.238.11:8087 check
    server  stapp03 172.16.238.12:8087 check

**5. Validate the haproxy configuration file by running the below command**

```
[root@stlb01 ~]# haproxy -f /etc/haproxy/haproxy.cfg
```

**6. Enable and start the haproxy service**

```
[root@stlb01 ~]# systemctl enable haproxy
Created symlink /etc/systemd/system/multi-user.target.wants/haproxy.service → /usr/lib/systemd/system/haproxy.service.

[root@stlb01 ~]# systemctl start haproxy

[root@stlb01 ~]# systemctl status haproxy
● haproxy.service - HAProxy Load Balancer
   Loaded: loaded (/usr/lib/systemd/system/haproxy.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2023-08-30 18:51:14 UTC; 12s ago
  Process: 1065 ExecStartPre=/usr/sbin/haproxy -f $CONFIG -f $CFGDIR -c -q $OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 1093 (haproxy)
    Tasks: 2 (limit: 1340692)
   Memory: 3.5M
   CGroup: /docker/8dcb74f08392ed71de8fcd6040d5c87a20c4673bb9c1a8fd24d2d20d3463d2a1/system.slice/haproxy.service
           ├─1093 /usr/sbin/haproxy -Ws -f /etc/haproxy/haproxy.cfg -f /etc/haproxy/conf.d -p /run/haproxy.pid
           └─1107 /usr/sbin/haproxy -Ws -f /etc/haproxy/haproxy.cfg -f /etc/haproxy/conf.d -p /run/haproxy.pid

Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1]: haproxy.service: About to execute: /usr/sbin/haproxy -Ws -f $CONFIG -f $CFGDIR -p $PIDFILE $OPTIONS
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1]: haproxy.service: Forked /usr/sbin/haproxy as 1093
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1093]: PR_SET_MM_ARG_START failed, proceeding without: Operation not permitted
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1]: haproxy.service: Changed start-pre -> start
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1093]: haproxy.service: Executing: /usr/sbin/haproxy -Ws -f /etc/haproxy/haproxy.cfg -f /etc/haproxy/conf.d -p 
/run/haproxy.pid
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1]: haproxy.service: Got notification message from PID 1093 (READY=1, MAINPID=1093)
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1]: haproxy.service: Changed start -> running
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1]: haproxy.service: Job haproxy.service/start finished, result=done
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1]: Started HAProxy Load Balancer.
Aug 30 18:51:14 stlb01.stratos.xfusioncorp.com systemd[1]: haproxy.service: Failed to send unit change signal for haproxy.service: Connection reset by peer

[root@stlb01 ~]# logout
[loki@stlb01 ~]$ logout
Connection to stlb01 closed.
```

**7. Validate the task  from jump server** 

```
thor@jump_host ~$ curl 172.16.238.14:80
Welcome to xFusionCorp Industries!

thor@jump_host ~$ curl 172.16.238.10:8087
Welcome to xFusionCorp Industries!

thor@jump_host ~$ curl 172.16.238.11:8087
Welcome to xFusionCorp Industries!

thor@jump_host ~$ curl 172.16.238.12:8087

Welcome to xFusionCorp Industries!
```




