

## Questions:

Nautilus developers are actively working on one of the project repositories, `(/usr/src/kodekloudrepos/demo)`. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:

1. On `Storage server` in Stratos DC create a new branch `xfusioncorp_demo` from `master` branch in `/usr/src/kodekloudrepos/demo` git repo.

2. Please do not try to make any changes in the code.



## Solution:   

**1. Login on storage server  & Switch Root user**

```

thor@jump_host ~$ ssh natasha@ststor01

natasha@ststor01's password: Bl@kW


[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**2. go to project repositories given in the task**

```

[root@ststor01 ~]# cd /usr/src/kodekloudrepos/demo
[root@ststor01 demo]# ls
data.txt  info.txt
```

**3. Checkout  to master, since its need to create a new branch from master**

```

[root@ststor01 demo]# git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

**4. Create a new branch from master as per the task**

```

[root@ststor01 demo]# git checkout -b xfusioncorp_demo
Switched to a new branch 'xfusioncorp_demo'
[root@ststor01 demo]# git status
On branch xfusioncorp_demo
nothing to commit, working tree clean
```

**5. validate the task by running**

```

[root@ststor01 demo]# git branch -a
  kodekloud_demo
  master
* xfusioncorp_demo
  remotes/origin/master
```

**6. Click on `Finish` & `Confirm` to complete the task successfully.**


