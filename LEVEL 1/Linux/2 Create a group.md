

## Questions:

There are specific access levels for users defined by the xFusionCorp Industries system admin team. Rather than providing access levels to every individual user, the team has decided to create groups with required access levels and add users to that groups as needed. See the following requirements:


a. Create a group named nautilus_noc in all App servers in Stratos Datacenter.

b. Add the user stark to nautilus_noc group in all App servers. (create the user if doesn't exist).

## Solution:  

**1. Login on  all App server**  

```
thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n
[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n
```


**2. Create Group named given in your task by below command**

`[root@stapp01 ~]# groupadd nautilus_noc`


**3. Check user is already present, if not then create user**

```
[root@stapp01 ~]# id stark
id: ‘stark’: no such user
[root@stapp01 ~]# cat /etc/passwd |grep stark
```

**4. Now create a user  adding in to newly created group**

```[root@stapp01 ~]# useradd -G nautilus_noc stark```


**5.  Validate the task by below commands**

```
[root@stapp01 ~]# id stark
uid=1002(stark) gid=1003(stark) groups=1003(stark),1002(nautilus_noc)
[root@stapp01 ~]# cat /etc/passwd |grep stark
stark:x:1002:1003::/home/stark:/bin/bash
```


`Please Note` :- I have showed only for stapp01. 
You have to do this in all app server stapp01,stapp02, stapp03. 

**6.Click on `Finish` & `Confirm` to complete the task successful**






