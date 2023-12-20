
## Questions
During the daily standup, it was pointed out that the timezone across Nautilus Application Servers in Stratos Datacenter doesn't match with that of the local datacenter's timezone, which is America/Argentina/San_Luis.

## solution

**1. At first login on App server   &  Switch to  root user**

```
thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password:Ir0nM@n

[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n
```

**2. Run Below command to  check existing Time Zone set for server**

```
[root@stapp01 ~]# timedatectl
```

**3. Help give you syntax to set the time zone & others**

```
[root@stapp01 ~]# timedatectl -h
```

**4. Run Below command to set the time zone  & verify it**

```
[root@stapp01 ~]# timedatectl set-timezone America/Blanc-Sablon
[root@stapp01 ~]# timedatectl

Please Note :- I have shown only for stapp01. 
You have to do this in all app server stapp01,stapp02, stapp03. 
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**




