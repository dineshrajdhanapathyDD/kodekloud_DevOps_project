

## Question

The Nautilus application development team was working on a git repository  `/opt/beta.git`  which is cloned under  `/usr/src/kodekloudrepos`  directory present on  `Storage server`  in  `Stratos DC`. The team want to setup a hook on this repository, please find below more details:  
  

  

-   Merge the  `feature`  branch into the  `master`  branch`, but before pushing your changes complete below point.  
      
    
-   Create a  `post-update`  hook in this git repository so that whenever any changes are pushed to the  `master`  branch, it creates a release tag with name  `release-2023-06-15`, where  `2023-06-15`  is supposed to be the current date. For example if today is  `20th June, 2023`  then the release tag must be  `release-2023-06-20`. Make sure you test the hook at least once and create a release tag for today's release.  
      
    
-   Finally remember to push your changes.


## Solution

**1. Login `Storage server`  in  `Stratos DC`.**
```

thor@jump_host ~$ ssh natasha@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:cCWu6pwfDfa0xKoc5azPcCF4oq0WmiCKU+eVUSB9ntc.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
natasha@ststor01's password: Bl@kW
```

**2. git repository  `/opt/beta.git`  which is cloned under  `/usr/src/kodekloudrepos`  directory present on  `Storage server`  in  `Stratos DC`.**
```
[natasha@ststor01 ~]$ cd /usr/src/kodekloudrepos/beta

[natasha@ststor01 beta]$ ls
feature.txt  info.txt
```

**3. Checkout master**
```
[natasha@ststor01 beta]$ sudo git checkout master

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for natasha: Bl@kW
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

**4. Merge the feature**
```
[natasha@ststor01 beta]$ sudo git merge feature
Updating 8f07111..f270ef9
Fast-forward
 feature.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 feature.txt
 ```

 **5. Create the post-update with permission**
 ```
[natasha@ststor01 beta]$ sudo touch /opt/beta.git/hooks/post-update
[natasha@ststor01 beta]$ sudo chmod +x /opt/beta.git/hooks/post-update
```

**6. Creates a release tag with name .** 
```
[natasha@ststor01 beta]$ sudo vi /opt/beta.git/hooks/post-update
[natasha@ststor01 beta]$ sudo cat /opt/beta.git/hooks/post-update
#!/bin/bash
cd /opt/beta.git
git_tag=release-$(date "+%Y-%m-%d")
git tag $git_tag
```

**7. Push to master**
```
[natasha@ststor01 beta]$ sudo git push
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To /opt/beta.git
   8f07111..f270ef9  master -> master
[natasha@ststor01 beta]$ 
```
![hook 1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/b619ae8a-e162-43e5-bbac-a3fde56a6d9a)


**8. Click on `Finish` & `Confirm` to complete the task successfully**

![hook2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/83aa4fa0-630f-497b-98c6-f42a9fdc8eff)