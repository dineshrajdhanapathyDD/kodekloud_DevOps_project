

## Questions:

DevOps team created a new Git repository last week; however, as of now no team is using it. The Nautilus application development team recently asked for a copy of that repo on (Storage server) in Stratos DC. Please clone the repo as per details shared below:



The repo that needs to be cloned is `/opt/news.git`


Clone this git repository under `/usr/src/kodekloudrepos` directory. Please do not try to make any changes in the repo.


## Solution: 

**1. At first login on to the storage server &  Switch to the root user** 

```
thor@jump_host ~$ ssh natasha@ststor01
natasha@ststor01's password:  Bl@kW

[natasha@ststor01 ~]$ sudo su -
[sudo] password for natasha: Bl@kW
```

**2. clone git repository under directory mentioned in your task**

```
[root@ststor01 ~]# cd /usr/src/kodekloudrepos
```

**3 . Run command  to clone**

```
[root@ststor01 kodekloudrepos]# git clone /opt/news.git
Cloning into 'news'...
warning: You appear to have cloned an empty repository.
done.
```

**4. Validate the task successfully by list the folder**

```
[root@ststor01 kodekloudrepos]# ls news
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**
