

## Questions:
After doing some security audits of servers, xFusionCorp Industries security team has implemented some new security policies. One of them is to disable direct root login through SSH.


Disable direct SSH root login on all app servers in Stratos Datacenter.


## Solution:  

**1. At first login to one of the App server  ssh tony@stapp01**

```
thor@jump_host /$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -

[sudo] password for tony: Ir0nM@n
```

**2.  Edit the  /etc/ssh/sshd_config  file and correct the changes as per below**

```
[root@stapp01 ~]# cat /etc/ssh/sshd_config  | grep PermitRoot
#PermitRootLogin yes
# the setting of "PermitRootLogin without-password".

 

Replace the    #PermitRootLogin yes  to    PermitRootLogin no

[root@stapp01 ~]# vi /etc/ssh/sshd_config

open another terminal shown like
#LoginGraceTime 2m
#PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

enter -i to edit
editable :
#LoginGraceTime 2m
PermitRootLogin no
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

use cltr+c to exit -
close terminal use :wq

[root@stapp01 ~]# cat /etc/ssh/sshd_config  | grep PermitRoot
PermitRootLogin no

# the setting of "PermitRootLogin without-password".
```

**3.  Restart service: systemctl restart sshd && systemctl status sshd**

```
[root@stapp01 ~]# systemctl restart sshd && systemctl status sshd


Please Note :- I have shown only for stapp01. 
You have to do this in all app server stapp01,stapp02, stapp03. 
```

**4.  Click on `Finish` & `Confirm` to complete the task successful**
