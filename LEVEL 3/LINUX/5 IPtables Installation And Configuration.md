

## Quesions:

We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 8082 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:



- Install iptables and all its dependencies on each app host.

- Block incoming port 8082 on all apps for everyone except for LBR host.

- Make sure the rules remain, even after system reboot.


## Solution:

**1. At first login on App server  ssh tony@stapp01**

```

thor@jump_host ~$ ssh tony@stapp01
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:TaYu/UH5bzGcrRoqCe2uq9afC+QqoNM5GXP186Aveq8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01,172.16.238.10' (ECDSA) to the list of known hosts.
tony@stapp01's password: Ir0nM@n

```

2. Switch to  root user : sudo su -

```
[tony@stapp01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: Ir0nM@n
```

**3. Run Below command to install IPtables service**

```
[root@stapp01 ~]# yum install  -y iptables-services
```

**4. Start & enable the IPtables service below commands**

```
[root@stapp01 ~]# systemctl start iptables 
 
[root@stapp01 ~]# systemctl enable iptables

[root@stapp01 ~]# systemctl status iptables
```

**5. Add below IPtables Rules as per your task ( Please check port no as per your task)**

```
[root@stapp01 ~]# iptables -A INPUT -p tcp --destination-port 8082 -s 172.16.238.14 -j ACCEPT
[root@stapp01 ~]# iptables -A INPUT -p tcp --destination-port 8082 -j DROP
```

**6. Add below IPtables Rules  for successful task completion**

```

[root@stapp01 ~] iptables -L --line-numbers

[root@stapp01 ~]# iptables -R INPUT 5 -p icmp -j REJECT
```

**7.  Finally saved the rules to ensure they remain persistent across reboot**

```

[root@stapp01 ~]# service iptables save
iptables: Saving firewall rules to /etc/sysconfig/iptables:[  OK  ]
```

**8.  For persistent across reboot restart IPtables service & validate**

```

[root@stapp01 ~]# systemctl restart iptables
[root@stapp01 ~]# systemctl status iptables
```

**9.  Validate the task by listing Iptables Rules post restart services.**

```

[root@stapp01 ~]# curl -I stapp01:8082
    
Telnet or Curl the Apache port 8082  ( Please check port no as per your task)
    From LB server it should be reachable & accessible

    Whereas  form Jump server it should not be accessible 


Please Note :- I have shown only for stapp01. You have to do this in all app server stapp01,stapp02, stapp03. 
```

**10.  Click on `Finish` & `Confirm` to complete the task successfully**