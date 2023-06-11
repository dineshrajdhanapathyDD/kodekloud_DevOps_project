

The Nautilus production support team and security team had a meeting last month in which they decided to use local yum repositories for maintaing packages needed for their servers. For now they have decided to configure a local yum repo on Nautilus Backup Server. This is one of the pending items from last month, so please configure a local yum repository on Nautilus Backup Server as per details given below.



a. We have some packages already present at location /packages/downloaded_rpms/ on Nautilus Backup Server.

b. Create a yum repo named epel_local and make sure to set Repository ID to epel_local. Configure it to use package's location /packages/downloaded_rpms/.

c. Install package httpd from this newly created repo.


Solution:  

1. At first login on Backup server  & Switch to  root user


thor@jump_host ~$ ssh clint@stbkp01

clint@stbkp01's password: H@wk3y3


[clint@stbkp01 ~]$ sudo su -

[sudo] password for clint: H@wk3y3


2. Run the Below command to   check is there  already any yum repository is available     

[root@stbkp01 ~]# yum repolist   
Loaded plugins: fastestmirror, ovl
Determining fastest mirrors
repolist: 0


3. Since there is no repo you need to create local repo  as per the task

[root@stbkp01 ~]# vi /etc/yum.repos.d/epel_local.repo


another terminal 
enter -i to insert
[epel_local]
name=epel_local
baseurl=file:///packages/downloaded_rpms/
enabled = 1
gpgcheck = 0

control+c to exit
:wq


[root@stbkp01 ~]# cat /etc/yum.repos.d/epel_local.repo
[epel_local]
name=epel_local
baseurl=file:///packages/downloaded_rpms/
enabled = 1
gpgcheck = 0

[root@stbkp01 ~] cd /etc/yum.repos.d/


4. After the file is saved, check to create repo listed 

[root@stbkp01 yum.repos.d]# yum repolist

repolist: 55

5. As per the task install package  (I have installed httpd using this repo )

[root@stbkp01 yum.repos.d]# yum install httpd

Complete!


6. Validate the task :
[root@stbkp01 yum.repos.d]# yum list httpd


Installed Packages
httpd.x86_64                      2.4.6-90.el7.centos                      @epel_local


7.  Click on Finish & Confirm to complete the task successful