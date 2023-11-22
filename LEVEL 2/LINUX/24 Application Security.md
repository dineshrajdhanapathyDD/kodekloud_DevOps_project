

Questions:
We have a backup management application UI hosted on Nautilus's backup server in Stratos DC. That backup management application code is deployed under Apache on the backup server itself, and Nginx is running as a reverse proxy on the same server. Apache and Nginx ports are 6000 and 8098, respectively. We have iptables firewall installed on this server. Make the appropriate changes to fulfill the requirements mentioned below:



We want to open all incoming connections to Nginx's port and block all incoming connections to Apache's port. Also make sure rules are permanent.


Solution:  
1. At first login on Backup server &  Switch to  root user 


thor@jump_host ~$ ssh clint@stbkp01
clint@stbkp01's password: H@wk3y3

[clint@stbkp01 ~]$ sudo su -

[sudo] password for clint: H@wk3y3

2. Run Below command to check Iptables  service installed and running status    

[root@stbkp01 ~]# systemctl status iptables

Active: inactive (dead)

3. Please check port no as per your task

[root@stbkp01 ~]# ss -tlnp |grep http



[root@stbkp01 ~]# ss -tlnp |grep nginx



4.  Start the httpd service & check the status.      

[root@stbkp01 ~]# systemctl start iptables

[root@stbkp01 ~]# systemctl status iptables

active

5. Add below IPtables Rules as per your task ( Please check port no as per your task)

[root@stbkp01 ~]# [root@stbkp01 ~]# iptables -A INPUT -p tcp --dport 8098 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT

[root@stbkp01 ~]# iptables -A INPUT -p tcp --dport 6000 -m conntrack --ctstate NEW -j REJECT


6.  There is by default Rule 5 to Reject all request check & change to ICMP

[root@stbkp01 ~]# iptables -L --line-numbers

[root@stbkp01 ~]# iptables -R INPUT 5 -p icmp -j REJECT


7. Finally saved the rules to ensure they remain persistent across reboot

 [root@stbkp01 ~]#service iptables save

[root@stbkp01 ~]# iptables -L --line-numbers

8.  Validate Ngnix port reachable from JumpServer as per the task request

another terminal :

thor@jump_host ~$ telnet stbkp01 6000

thor@jump_host ~$ telnet stbkp01 8098


9.  Click on Finish & Confirm to complete the task successful