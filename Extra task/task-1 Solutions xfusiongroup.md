



## Question : 
There are new requirements to automate a backup process that was performed manually by the xFusionCorp Industries system admins team earlier. To automate this task, the team has developed a new bash script xfusioncorp.sh. They have already copied the script on all required servers, however they did not make it executable on one the app server i.e App Server 1 in Stratos Datacenter.

Please give executable permissions to `/tmp/xfusioncorp.sh` script on `App Server 1`. Also make sure every user can execute it.



## Solution:  

**1. Login on   App server as per the task**

```
ssh banner@stapp03
password  : BigGr33n

sudo su -
password :  BigGr33n
```

**2. List the file existing file permission**    

```
[root@stapp03 ~]# ls -ltr /tmp/xfusioncorp.sh

---------- 1 root root 40 Jun 17 09:02 /tmp/xfusioncorp.sh  

ll -lsd /tmp/xfusioncorp.sh
```

**3. As per the task all other users need to have execute permission**  

```
[root@stapp03 ~]# chmod o+rx /tmp/xfusioncorp.sh

Please note that in case of bash script bash is the interpreter that is actually going to execute the script and the interpreter needs to read the script so even if you have given it only executable permission the interpreter i.e bash will not be able to execute it so you had to give it read permission as well along with execute permission.

[root@stapp03 ~]# cat /tmp/xfusioncorp.sh
```

**4. Verify the file permissions and execute the script form user**

```
ll -lsd /tmp/xfusioncorp.sh  or 
[root@stapp01 ~]#ls -lrt /tmp/xfusioncorp.sh or 
[root@stapp03 ~]#ls -lrt /tmp/xfusioncorp.sh

-------r-x 1 root root 40 Jun 17 09:25 /tmp/xfusioncorp.sh 

[root@stapp03 ~]# exit

logout

[banner@stapp01 ~]$ sh /tmp/xfusioncorp.sh

Welcome To KodeKloud
```

**5. Click on `Finish` & `Confirm` to complete the task successful**


exit

cd /tmp/ 

sh xfusioncorp.sh


Welcome To KodeKloud


