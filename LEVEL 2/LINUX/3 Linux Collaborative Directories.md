
## Questions:

The Nautilus team doesn't want its data to be accessed by any of the other groups/teams due to security reasons and want their data to be strictly accessed by the sysops group of the team.



Setup a collaborative directory `/sysops/data` on Nautilus App 2 server in Stratos Datacenter.

The directory should be group owned by the group sysops and the group should own the files inside the directory. The directory should be `read/write/execute` to the group owners, and others should not have any access.


## Solutions:

  
**1. Login on   App server as per the task**

```

thor@jump_host /$ ssh steve@stapp02

steve@stapp02's password: Am3ric@


[tony@stapp02 ~]$ sudo su -

password: Am3ric@
```

**2. Create folder according to the task & list to confirm**

```

[root@stapp02 ~]# mkdir -p /sysops/data 
[root@stapp02 ~]# ll -lsd /sysops/data/
```

**3. Change group of the directory from root to mentioned group in task**

- chgrp = changing primary group for the directory.

-R = recursive . changes will reflect all sub-directories and files below.
```
[root@stapp02 ~]# chgrp -R sysops /sysops/data

[root@stapp02 ~]# ll -lsd /sysops/data/
```

**4. Change group of the directory from root to mentioned group in task**


```
chmod - modifying permission for sharedgrp.

2770  - Giving full permission to user (root) & group users and zero permmision for other users.

           2 - Special Permission. set group id.

           7 -  4+2+1 = 7 (4= read, 2 = write, 1 = execute).User (root) Permission

           7 -  4+2+1 = 7 (4= read, 2 = write, 1 = execute). Group Permission


           0 - Other users.(zero permission).



[root@stapp02 ~]# chmod -R 2770 /sysops/data

[root@stapp02 ~]# ll -lsd /sysops/data/
```

**5. Click on `Finish` & `Confirm` to complete the task successful**

