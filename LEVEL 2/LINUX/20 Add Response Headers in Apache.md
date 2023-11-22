

Questions:

We are working on hardening Apache web server on all app servers. As a part of this process we want to add some of the Apache response headers for security purpose. We are testing the settings one by one on all app servers. As per details mentioned below enable these headers for Apache:



Install httpd package on App Server 2 using yum and configure it to run on 3003 port, make sure to start its service.

Create an index.html file under Apache's default document root i.e /var/www/html and add below given content in it.

Welcome to the xFusionCorp Industries!

Configure Apache to enable below mentioned headers:

X-XSS-Protection header with value 1; mode=block

X-Frame-Options header with value SAMEORIGIN

X-Content-Type-Options header with value nosniff

Note: You can test using curl on the given app server as LBR URL will not work for this task.


Solution:  
1. Login on   App server as per the task & switch to root user

thor@jump_host ~$ ssh steve@stapp02

steve@stapp02's password: Am3ric@

[steve@stapp02 ~]$ sudo su -
[sudo] password for steve: Am3ric@


2. Install httpd package on the server

[root@stapp02 ~]# yum install httpd -y

3. Edit the configuration  file change port as per the task & add Header at end
 
[root@stapp02 ~]# rpm -qa |grep httpd
[root@stapp02 ~]# cat /etc/httpd/conf/httpd.conf |grep Listen
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on specific IP addresses as shown below to 
#Listen 12.34.56.78:80
Listen 80

[root@stapp02 ~]# vi /etc/httpd/conf/httpd.conf
another terminal

#Listen 12.34.56.78:80
Listen 80 - change "3003"

IncludeOtional conf,d/*.conf

enter the below setup:
Header set X-XSS-Protection "1; mode=block"
Header always append X-Frame-Options SAMEORIGIN
Header set X-Content-Type-Options nosniff


control +c exit as :wq

change as Listen =3003
[root@stapp02 ~]# cat /etc/httpd/conf/httpd.conf |grep Listen
# Listen: Allows you to bind Apache to specific IP addresses and/or
# Change this to Listen on specific IP addresses as shown below to 
#Listen 12.34.56.78:80
Listen 3003 

header file also added in httpd:
[root@stapp02 ~]# cat /etc/httpd/conf/httpd.conf  |grep X
Header set X-XSS-Protection "1; mode=block"
Header always append X-Frame-Options SAMEORIGIN
Header set X-Content-Type-Options nosniff

4. Create Index file with given content in task
[root@stapp02 ~]# ll /var/www/html/
total 0

[root@stapp02 ~]# vi /var/www/html/index.html

another terminal add file:
Welcome to the xFusionCorp Industries!

press -control+c 
exit :wq


[root@stapp02 ~]# cat /var/www/html/index.html
Welcome to the xFusionCorp Industries!

5. Start httpd & check the status
[root@stapp02 ~]# systemctl start httpd
[root@stapp02 ~]# systemctl status httpd

execueted as : active running status

6. Validate the task by Curl
[root@stapp02 ~]# curl http://localhost:3003
Welcome to the xFusionCorp Industries!

[root@stapp02 ~]# curl -i http://localhost:3003

7. Click on Finish & Confirm to complete the task successful.

