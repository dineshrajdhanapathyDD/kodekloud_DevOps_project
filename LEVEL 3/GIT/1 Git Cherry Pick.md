

## Questions:

The Nautilus application development team has been working on a project repository `/opt/apps.git`. This repo is cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`. They recently shared the following requirements with the DevOps team:



There are two branches in this repository, `master` and `feature`. One of the developers is working on the `feature` branch and their work is still in progress, however they want to merge one of the commits from the `feature` branch to the `master` branch, the message for the commit that needs to be merged into `master` is `Update info.txt`. Accomplish this task for them, also remember to push your changes eventually.



## Solution:  


**1. At first login on to the storage server  & switch to the root user**

```

thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:VhEXXutGnMVCWBmQn+3nfE3f46LipcT8wUzWrwaSkpI.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
natasha@ststor01's password: Bl@kW
[natasha@ststor01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for natasha: Bl@kW
[root@ststor01 ~]# 
```

**2. Check repo and branch status**

```

[root@ststor01 kodekloudrepos]# cd /usr/src/kodekloudrepos/apps/
[root@ststor01 apps]# git branch
* feature
  master
```

**3. Git log and find the text file**

```

[root@ststor01 apps]# git log --oneline |grep "Update info.txt"
61baec7 Update info.txt
[root@ststor01 apps]# git log --oneline
7184404 (HEAD -> feature, origin/feature) Update welcome.txt
61baec7 Update info.txt
d9c1e16 (origin/master, master) Add welcome.txt
7432356 initial commit
```

**4. Git Checkout master & merge to new branch** 

```

[root@ststor01 apps]# git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
[root@ststor01 apps]# git branch
  feature
* master
```

**5. check the cherry-pick**

```

[root@ststor01 apps]# git cherry-pick 61baec7
[master 149cd6f] Update info.txt
 Date: Fri Sep 22 19:44:51 2023 +0000
 1 file changed, 1 insertion(+), 1 deletion(-)
```

**6. Git push to the branch as well master**

```

[root@ststor01 apps]# git push origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 36 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 315 bytes | 315.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/apps.git
   d9c1e16..149cd6f  master -> master
```

**8. Click on `Finish` & `Confirm` to complete the task successfully**