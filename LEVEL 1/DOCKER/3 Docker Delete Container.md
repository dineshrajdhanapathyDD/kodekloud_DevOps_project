

## Questions:

One of the Nautilus project developers created a container on `App Server 3`. This container was created for testing only and now we need to delete it. Accomplish this task as per details given below:

Delete a container named `kke-container`, its running on `App Server 3` in Stratos DC.


## Solutions:

**1. Login on app server given in task and switch the root.**

```
thor@jump_host ~$ ssh banne@sapp03
banner@stapp03's password:  BigGr33n

[banner@stapp03 ~]$ sudo su -
[sudo] password for banner: BigGr33n
```

**2. Confirm docker container is running.**

```
CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS              PORTS               NAMES
e7bbbffc146a        busybox             "tail -f /dev/null"   4 minutes ago       Up 4 minutes                            kke-container
```

**3. As per task stop the conainer**

```
[root@stapp03 ~]# docker stop kke-container
kke-container
```

**4. As per task remove the container.**

```
[root@stapp03 ~]# docker rm kke-container
kke-container
[root@stapp03 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

**5.  Click on `Finish` & `Confirm` to complete the task successfully.**