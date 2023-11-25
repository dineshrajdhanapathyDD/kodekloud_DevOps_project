

## Questions:

`xFusionCorp Industries` has an application running on `Nautlitus` infrastructure in Stratos Datacenter. The monitoring tool recognised that there is an issue with the `haproxy` service on `LBR` server. That needs to fixed to make the application work properly.

Troubleshoot and fix the issue, and make sure `haproxy` service is running on `Nautilus LBR` server. Once fixed, make sure you are able to access the website using `StaticApp` button on the top bar.


## Solution:

**1. At first login to one of the App server  &  Switch to  root user**

```
thor@jump_host ~$ ssh loki@stlb01
loki@stlb01's password: Mischi3f

[loki@stlb01 ~]$ sudo su -
[sudo] password for loki: Mischi3f
```

**2.  Verify the status of haproxy service**

```
[root@stlb01 ~]# systemctl status haproxy
● haproxy.service - HAProxy Load Balancer
   Loaded: loaded (/usr/lib/systemd/system/haproxy.service; disabled; vendor preset: disabled)
   Active: inactive (dead)

Aug 31 11:14:16 stlb01.stratos.xfusioncorp.com systemd[1]: Collecting haproxy.service
```

**3. First validate the existing the haproxy config file using the below command**   

```
[root@stlb01 ~]# haproxy -c -f /etc/haproxy/haproxy.cfg
[ALERT] 242/111456 (359) : Proxy 'main': unable to find required default_backend: 'app'.
[ALERT] 242/111456 (359) : Fatal errors found in configuration.
[root@stlb01 ~]# cat /etc/haproxy/haproxy.cfg |grep app
    default_backend             app
#backend app
    server  app1 stapp01:8087 check
    server  app2 stapp02:8087 check
    server  app3 stapp03:8087 check
```

**4. Correct the typo error in the file haproxy.cfg** 

```
[root@stlb01 ~]# vi /etc/haproxy/haproxy.cfg
[root@stlb01 ~]# cat /etc/haproxy/haproxy.cfg
global
    log         127.0.0.1 local2

    chroot      /var/lib/haproxy
    pidfile     /var/run/haproxy.pid
    maxconn     4000
    user        haproxy
    group       haproxy
    daemon

    # turn on stats unix socket
    stats socket /var/lib/haproxy/stats

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
frontend  main *:80
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
    server  app1 stapp01:8087 check
    server  app2 stapp02:8087 check
    server  app3 stapp03:8087 check
```

**5. After changes  validate the existing the haproxy config file**

```
[root@stlb01 ~]# haproxy -c -f /etc/haproxy/haproxy.cfg
Configuration file is valid
```

**6. Start service &   Check service status**

```
[root@stlb01 ~]# systemctl start haproxy
[root@stlb01 ~]# systemctl status haproxy
● haproxy.service - HAProxy Load Balancer
   Loaded: loaded (/usr/lib/systemd/system/haproxy.service; disabled; vendor preset: disabled)
   Active: active (running) since Thu 2023-08-31 11:18:51 UTC; 12s ago
 Main PID: 368 (haproxy-systemd)
   CGroup: /docker/ec8137d76afe440ab9d786f40cdb1f44fa60676e614adf9a6e43e9f6f95f7c46/system.slice/haproxy.service
           ├─368 /usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid
           ├─369 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
           └─370 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds

Aug 31 11:18:51 stlb01.stratos.xfusioncorp.com systemd[1]: About to execute: /usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid $OPTIONS
Aug 31 11:18:51 stlb01.stratos.xfusioncorp.com systemd[1]: Forked /usr/sbin/haproxy-systemd-wrapper as 368
Aug 31 11:18:51 stlb01.stratos.xfusioncorp.com systemd[1]: haproxy.service changed dead -> running
Aug 31 11:18:51 stlb01.stratos.xfusioncorp.com systemd[1]: Job haproxy.service/start finished, result=done
Aug 31 11:18:51 stlb01.stratos.xfusioncorp.com systemd[1]: Started HAProxy Load Balancer.
Aug 31 11:18:51 stlb01.stratos.xfusioncorp.com systemd[368]: Executing: /usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid
Aug 31 11:18:51 stlb01.stratos.xfusioncorp.com haproxy-systemd-wrapper[368]: haproxy-systemd-wrapper: executing /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
```

**7. Click on `Finish` & `Confirm` to complete the task successful**



