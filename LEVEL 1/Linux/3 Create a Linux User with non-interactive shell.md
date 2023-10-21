

## Questions:

The System admin team of xFusionCorp Industries has installed a backup agent tool on all app servers. As per the tool's requirements they need to create a user with a non-interactive shell.
Therefore, create a user named yousuf with a non-interactive shell on the App Server 2


## Solutions

**1. At first login to the respective mentioned server in the task. Mine is stapp02**

```
thor@jump_host ~$ ssh steve@stapp02
password : Am3ric@
```


**2. 1st check user is existed on the server  by below command** 
```
[steve@stapp02 ~]$ sudo id yousuf
pasword : Am3ric@
```


**3.  If the user is not found the then you create a user with a non-interactive shell** 
```
[steve@stapp02 ~]$ sudo useradd yousuf -s /sbin/nologin
[steve@stapp02 ~]$ id yousuf
```


**4.  Validate user is created successfully as per the task request.** 

```
[steve@stapp02 ~]$ id yousuf 
uid=1002(yousuf) gid=1002(yousuf) groups=1002(yousuf)

[steve@stapp02 ~]$ cat /etc/passwd |grep yousuf
yousuf:x:1002:1002::/home/yousuf:/sbin/nologin
```


**5.  Click on Finish & Confirm to complete the task successful.**


`Refer below credentials;`
    

```console
Purpose -Application 1
Server Name  -   stapp01
IP  -  172.16.238.10
Hostname   -   stapp01.stratos.xfusioncorp.com
User  -  tony
Password  -  Ir0nM@n

 
Purpose - Application 2
Server Name  - stapp02
IP  -   172.16.238.11
Hostname   -    stapp02.stratos.xfusioncorp.com
User  -steve
Password  -  Am3ric@
 
Purpose -  Application 3
Server Name  - stapp03
IP  -   172.16.238.12
Hostname   -    stapp03.stratos.xfusioncorp.com
User  - banner
Password  -    BigGr33n
```