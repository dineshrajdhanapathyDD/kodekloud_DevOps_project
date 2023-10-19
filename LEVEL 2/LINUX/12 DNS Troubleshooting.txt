

Quesions:
The system admins team of xFusionCorp Industries has noticed intermittent issues with DNS resolution in several apps . App Server 1 in Stratos Datacenter is having some DNS resolution issues, so we want to add some additional DNS nameservers on this server.



As a temporary fix we have decided to go with Google public DNS (ipv4). Please make appropriate changes on this server.



Solution:  
1. At first login to app server  & Switch to  root user 
thor@jump_host ~$ ssh tony@stapp01

tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -

[sudo] password for tony: Ir0nM@n

3.  Verify the existing  resolve.conf file
[root@stapp01 ~]# cat /etc/resolv.conf
search stratos.xfusioncorp.com
nameserver 127.0.0.11
options ndots:0


4.  Edit the  resolv.conf  file and add public DNS 8.8.8.8 as per task.

[root@stapp01 ~]# vi /etc/resolv.conf
enter another terminal:
search stratos.xfusioncorp.com
nameserver 127.0.0.11
options ndots:0

we insert nameserver 8.8.8.8 -i

search stratos.xfusioncorp.com
nameserver 127.0.0.11
nameserver 8.8.8.8
options ndots:0

control+c
exit :wq
 

[root@stapp01 ~]# cat /etc/resolv.conf
search stratos.xfusioncorp.com
nameserver 127.0.0.11
nameserver 8.8.8.8
options ndots:0

5.  Click on Finish & Confirm to complete the task successful