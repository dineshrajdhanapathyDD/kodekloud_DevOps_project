


Questions:
The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

a. Install cronie package on all Nautilus app servers and start crond service.

b. Add a cron */5 * * * * echo hello > /tmp/cron_text for root user.





Solution:  
1. At first login to all App server  & Switch to  root user


thor@jump_host ~$ ssh tony@stapp01
 tony@stapp01's password:    Ir0nM@n

[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n


2. Install cronie package on server 


[root@stapp01 ~]# yum install cronie -y


3.  Start cron service & check the  status 


[root@stapp01 ~]# systemctl start crond.service

[root@stapp01 ~]# systemctl status crond.service

4.  Create a cronjob  as per the task for root user
    
[root@stapp01 ~]# crontab -e

open new terminal - insert  "-i" then paste "*/5 * * * * echo hello > /tmp/cron_text" and crl+c :wq - return to default terminal.

5.  Check cron job for user root

[root@stapp01 ~]# crontab -l
*/5 * * * * echo hello > /tmp/cron_text

6.  Validate  cron_text file is created successfully, before that done finish the task

[root@stapp01 ~]# ll /tmp/

total 8

-rw-r--r-- 1 root root   6 Jun 23 09:55 cron_text

-rwx------ 1 root root 836 Aug  1  2019 ks-script-rnBCJB

-rw------- 1 root root   0 Aug  1  2019 yum.log

Please Note :- I have showed only for stapp01. 
You have to do this in all app server stapp01,stapp02, stapp03. 

stapp02 172.16.238.11 stapp02.stratos.xfusioncorp.com steve(server name2)    Am3ric@(password)  Application 2

stapp03 172.16.238.12 stapp03.stratos.xfusioncorp.com banner(servername3) BigGr33n(password) Application 3

7.  Click on Finish & Confirm to complete the task successful



