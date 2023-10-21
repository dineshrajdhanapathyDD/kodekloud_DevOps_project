

## Questions:

A developer ammar has been assigned Nautilus project temporarily as a backup resource. As a temporary resource for this project, we need a temporary user for ammar. It’s a good idea to create a user with a set expiration date so that the user won't be able to access servers beyond that point.

Therefore, create a user named ammar on the App Server 1. Set expiry date to 2021-03-28 in Stratos Datacenter. Make sure the user is created as per standard and is in lowercase.

## Solution: 

**1. At first login on App server   &  Switch to  root user** 

```
thor@jump_host ~$ ssh tony@stapp01

tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -

[sudo] password for tony: Ir0nM@n
```


**2.  1st check user is existed on the server  by below command** 

```
[root@stapp01 ~]# id ammar
id: ammar: no such user
```


**3.  If user not found the then you create a user  as per given in your task** 

```
[root@stapp01 ~]# adduser ammar

[root@stapp01 ~]# id ammar
uid=1002(ammar) gid=1002(ammar) groups=1002(ammar)
```


**4.  Validate user is created successfully  with default expiry date** 

```
[root@stapp01 ~]# chage -l ammar

Last password change                                    : Jun 02, 2023
Password expires                                        : never
Password inactive                                       : never
(Account expires                                         : never) - expiry date not mentioned.
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```


**5. Run below command to change the default expires to date as per your task**

```
[root@stapp01 ~]# chage -E 2021-03-28 ammar
```


**6. Validate changes are implemented  successfully as per the task request.**

```
[root@stapp01 ~]# chage -l ammar

Last password change                                    : Jun 02, 2023
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
[root@stapp01 ~]# chage -E 2021-03-28 ammar
[root@stapp01 ~]# chage -l ammar
Last password change                                    : Jun 02, 2023
Password expires                                        : never
Password inactive                                       : never
(Account expires                                         : Mar 28, 2021)- expiry mentioned as per questions.
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7  
```


**7.  Click on Finish & Confirm to complete the task successful**


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