

## Questions:
As per new application requirements shared by the Nautilus project development team, serveral new packages need to be installed on all app servers in Stratos Datacenter. Most of them are completed except for epel-release.



Therefore, install the epel-release package on all app-servers.

## Solution:  

**1. At first login on App server  & Switch to  root user**

```

thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n
[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n
```

**2. Run the Below command  to check &  install epel-release package**       

```
[root@stapp01 ~]# rpm -qa |grep epel-release

[root@stapp01 ~]# yum install epel-release -y
```

**3. Validate the task by confirming**
```
[root@stapp01 ~]# rpm -qa |grep epel-release
```

**4. For  another app server without login you can execute the script**

```
thor@jump_host ~$ ssh -t steve@stapp02 "sudo yum install epel-release -y "

thor@jump_host ~$ ssh -t steve@stapp02 "sudo rpm -qa |grep epel-release "
```

**5. Run the script for  stapp03 from jump server**

```
thor@jump_host ~$ ssh -t banner@stapp03 "sudo yum install epel-release -y "

thor@jump_host ~$ ssh -t banner@stapp03 "sudo rpm -qa |grep epel-release "
```

**6.  Click on `Finish` & `Confirm` to complete the task successful**