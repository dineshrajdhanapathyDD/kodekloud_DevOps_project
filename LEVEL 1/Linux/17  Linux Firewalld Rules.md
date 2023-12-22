

## Questions:
The `Nautilus` system admins team recently deployed a web UI application for their backup utility running on the `Nautilus backup server` in `Stratos Datacenter`. The application is running on port `8087`. They have `firewalld` installed on that server. The requirements that have come up include the following:

- Open all incoming connection on `8087/tcp` port. Zone should be `public`.



## Solution:  

**1. Login  to respective mentioned Backup server in task & switch to root**


```
thor@jump_host /$ ssh clint@stbkp01
clint@stbkp01's password: H@wk3y3

[clint@stbkp01 ~]$ sudo su -
[sudo] password for clint: H@wk3y3
```

**2.  1st check firewalld service status and existing rules for public zone**

```
[root@stbkp01 ~]# firewall-cmd --zone=public --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces: 
  sources: 
  services: dhcpv6-client ssh
  ports: 
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules:
```

**3.  According to your task check the application port and add the below rule.**

```
[root@stbkp01 ~]# firewall-cmd  --permanent --zone=public --add-port=8087/tcp
success
```

**4.  Reload the firewall rules & restart the firewall services.**

```
[root@stbkp01 ~]# firewall-cmd --reload
success
[root@stbkp01 ~]# systemctl restart firewalld
[root@stbkp01 ~]# 
```

**5.  Validate firewall rule implemented successfully as per the task request**  

```
[root@stbkp01 ~]# firewall-cmd --zone=public --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces: 
  sources: 
  services: dhcpv6-client ssh
  ports: 8087/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules:
```

**6.  Click on `Finish` & `Confirm` to complete the task successful**



