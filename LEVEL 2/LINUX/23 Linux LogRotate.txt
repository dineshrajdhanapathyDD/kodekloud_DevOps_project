
Questions:
The Nautilus DevOps team is ready to launch a new application, which they will deploy on app servers in Stratos Datacenter. They are expecting significant traffic/usage of httpd on app servers after that. This will generate massive logs, creating huge log files. To utilise the storage efficiently, they need to compress the log files and need to rotate old logs. Check the requirements shared below:



a. In all app servers install httpd package.

b. Using logrotate configure httpd logs rotation to monthly and keep only 3 rotated logs.

(If by default log rotation is set, then please update configuration as needed)

Solution:  


1. Login on all app servers & switch to root user1. Login on   App server as per the task

thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -

[sudo] password for tony: Ir0nM@n


2. Install the package as per the given task

 [root@stapp01 ~]# yum install httpd -y


3. Navigate to logrotate folder and check existing folder

[root@stapp01 ~]# ll /etc/logrotate.d/

[root@stapp01 ~]# cat /etc/logrotate.d/httpd


4. As per the task edit log file & save the file


[root@stapp01 ~]# vi /etc/logrotate.d/httpd

open another terminal we add 'monthly' 'rotate 3'
/var/log/httpd/*log {
    monthly
    rotate 3
    missingok
    notifempty
    sharedscripts
    delaycompress
    postrotate
        /bin/systemctl reload httpd.service > /dev/null 2>/dev/null || true
    endscript
}

control+c exit :wq


[root@stapp01 ~]# cat /etc/logrotate.d/httpd
/var/log/httpd/*log {
    monthly
    rotate 3
    missingok
    notifempty
    sharedscripts
    delaycompress
    postrotate
        /bin/systemctl reload httpd.service > /dev/null 2>/dev/null || true
 

5. Start services & check the status


[root@stapp01 ~]# systemctl start httpd

[root@stapp01 ~]# systemctl status httpd

Please Note:- I have shown only for stapp01. 
You have to do this in all app server stapp01- Ir0nM@n,stapp02-Am3ric@, stapp03-BigGr33n. 


6.  Click on Finish & Confirm to complete the task successfully