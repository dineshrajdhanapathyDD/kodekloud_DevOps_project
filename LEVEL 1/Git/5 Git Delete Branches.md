

## Questions:

Nautilus developers are actively working on one of the project repositories, `/usr/src/kodekloudrepos/news`. They were doing some testing and created few test branches, now they want to clean those test branches. Below are the requirements that have been shared with the DevOps team:



On `Storage server` in Stratos DC delete a branch named `xfusioncorp_news` from `/usr/src/kodekloudrepos/news` git repo.


## Solution:  

**1. At first login on to the storage server &  Switch to the root user** 

```
thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password: Bl@kW

[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**2. Check the branch in git repository under directory mentioned in your task**

```
[root@ststor01 ~]# cd /usr/src/kodekloudrepos/news
[root@ststor01 news]# git branch
  master
* xfusioncorp_news
```

**3. switch to another branch under directory mentioned in your task:**

```
[root@ststor01 demo]# git checkout xfusioncorp_demo
Already on 'xfusioncorp_demo'
[root@ststor01 demo]# git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

**4. check the branch  and delete a branch named (xfusioncorp_news):**

```
[root@ststor01 demo]# git branch -a
* master
  xfusioncorp_demo
  remotes/origin/master
[root@ststor01 demo]# git branch -d xfusioncorp_demo
Deleted branch xfusioncorp_demo (was ce66695).
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**


















(error message : find the proper command check before-
[natasha@ststor01 news]$ git branch -d xfusioncorp_news
fatal: detected dubious ownership in repository at '/usr/src/kodekloudrepos/news'
To add an exception for this directory, call:

        git config --global --add safe.directory /usr/src/kodekloudrepos/news
[natasha@ststor01 news]$ git config --global --add safe.directory /usr/src/kodekloudrepos/news)

[natasha@ststor01 news]$ git push /opt/news.git --delete xfusioncorp_news

[natasha@ststor01 news]$ git push /opt/news.git
Everything up-to-date)


