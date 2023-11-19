

## Questions:

The Nautilus application development team has been working on a project repository `/opt/media.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`. They recently shared the following requirements with DevOps team:



Create a new branch `devops` in `/usr/src/kodekloudrepos/media` repo from `master` and copy the `/tmp/index.html` file (present on `storage server` itself) into the repo. Further, `add/commit` this file in the new branch and merge back that branch into `master` branch. Finally, push the changes to the origin for both of the branches.


## Solution:

**1. At first login on to the storage server  & switch to the root user**

```

thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW

[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**2. Check repo  git status**

```

[root@ststor01 ~]# ls /usr/src/kodekloudrepos/media/
info.txt  welcome.txt
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/media/
[root@ststor01 media]# git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

**3. Create a new branch as per the task from the master repo**

```

[root@ststor01 media]# git checkout -b devops
Switched to a new branch 'devops'
[root@ststor01 media]# git status
On branch devops
nothing to commit, working tree clean
```

**4. Check  new branch status  & copy the index file**

```

[root@ststor01 media]# git branch
* devops
  master
[root@ststor01 media]# cp /tmp/index.html  /usr/src/kodekloudrepos/media/
[root@ststor01 media]# ls /usr/src/kodekloudrepos/media/
index.html  info.txt  welcome.txt
```

**5. Git add , Commit Index.html file**

```

[root@ststor01 media]# git add index.html
[root@ststor01 media]# git commit -m "add media"
[devops 4a25899] add media
 1 file changed, 1 insertion(+)
 create mode 100644 index.html
```

**6. Git Checkout master & merge to new branch**

```

[root@ststor01 media]# git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
[root@ststor01 media]# git merge devops
Updating b6ab3a2..4a25899
Fast-forward
 index.html | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 index.html
```

**7. Git push to the branch as well master**

```

[root@ststor01 media]# git push -u origin devops
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 36 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 325 bytes | 325.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/media.git
 * [new branch]      devops -> devops
branch 'devops' set up to track 'origin/devops'.
[root@ststor01 media]# git push -u origin master
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/media.git
   b6ab3a2..4a25899  master -> master
branch 'master' set up to track 'origin/master'.
[root@ststor01 media]# git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

**8. Click on `Finish` & `Confirm` to complete the task successfully.**
