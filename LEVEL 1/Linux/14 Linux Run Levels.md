


## Question:

New tools have been installed on the app server in Stratos Datacenter. Some of these tools can only be managed from the graphical user interface. Therefore, there are requirements for these app servers.



On all App servers in Stratos Datacenter change the default runlevel so that they can boot in GUI (graphical user interface) by default. Please do not try to reboot these servers


**1. At first login on all app servers   & Switch to  root user**

```
thor@jump_host ~$ ssh tony@stapp01
password : Ir0nM@n


[tony@stapp01 ~]$ sudo su -
password : Ir0nM@n
```

**2. Run Below command to check the default run level**

```
[root@stapp01 ~]# systemctl get-default
```

**3. Run Below command to change the run level to graphical.target**

```
[root@stapp01 ~]# systemctl set-default graphical.target
```

**5. Post set default start graphical service & validate  the status**

```
[root@stapp01 ~]# systemctl status graphical.target

systemctl start graphical.target
systemctl start graphical.target
[root@stapp01 ~]# systemctl status graphical.target
[root@stapp01 ~]# systemctl get-default


Please Note :- I have showed only for stapp01. 
You have to do this in all app server  stapp01,stapp02, stapp03. 
```

**6.  Click on `Finish` & `Confirm` to complete the task successful**
