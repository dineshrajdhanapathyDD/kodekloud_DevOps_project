


Questions:
As per details shared by the development team, the new application release has some dependencies on the back end. There are some packages/services that need to be installed on all app servers under Stratos Datacenter. As per requirements please perform the following steps:



a. Install nscd package on all the application servers.

b. Once installed, make sure it is enabled to start during boot.


Solution:  
1. At first login on storage server  ssh tony@stapp01

2. Switch to  root user : sudo su -

3. Run Below command to install IPtables service 

    yum install  -y nscd


4. Start & enable the squid service below commands

    systemctl start nscd 

    systemctl enable nscd

5. Validate the squid service status   systemctl status nscd

Please Note :- I have showed only for stapp01. You have to do this in all app server stapp01,stapp02, stapp03. 

6.  Click on Finish & Confirm to complete the task successful
