

Question :   
There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database. After digging into the issue, the team found that mariadb service is down on the database server.

Look into the issue and fix the same.


Solution:  
1. At first login on database server  &  Switch to  root user 


thor@jump_host ~$ ssh peter@stdb01

peter@stdb01's password: Sp!dy

[peter@stdb01 ~]$ sudo su -
 
[sudo] password for peter: Sp!dy

3. When you start the mariadb service you will get below error 

[root@stdb01 ~]# systemctl status mariadb

[root@stdb01 ~]# systemctl start  mariadb


4. To know the detail error run status -l 

[root@stdb01 ~]# systemctl status mariadb -l

5. List the  MY SQL following path check ownership, groups and directory name

[root@stdb01 ~]# ll  /var/lib/


5. If the Ownership is correct check the folder name & rename accordingly


check the root of database server name :
drwxr-xr-x 1 mysql mysql 4096 May 25 13:09 mysql

[root@stdb01 ~]# mv /var/lib/mysql/ /var/lib/mysqld

check the root of database server name changed as mysqld :
drwxr-xr-x 1 mysql mysql 4096 May 25 13:09 mysqld

[root@stdb01 ~]# ll /var/lib/

6. Start service: systemctl start  mariadb &&  systemctl status  mariadb

[root@stdb01 ~]# systemctl start mariadb

[root@stdb01 ~]# systemctl status  mariadb

7. Click on Finish & Confirm to complete the task successful










6. Start service: systemctl start  mariadb &&  systemctl status  mariadb


another method:
[root@stdb01 ~]# ll  /var/lib/mysql
total 28704

[root@stdb01 ~]# ll  /var/lib/
total 52

[root@stdb01 ~]# chown mysql:mysql /var/run


[root@stdb01 ~]# ll /var/run/mariadb

[root@stdb01 ~]# ll -lsd /var/run/mariadb



[root@stdb01 ~]# systemctl start  mariadb 

[root@stdb01 ~]# systemctl status  mariadb
