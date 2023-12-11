

## Questions:

A new developer just joined the Nautilus development team and has been assigned a new project for which he needs to create a new repository under his account on Gitea server. Additionally, there is some existing data that need to be added to the repo. Below you can find more details about the task:

Click on the `Gitea UI` button on the top bar. You should be able to access the `Gitea` UI. Login to `Gitea` server using username `max` and password `Max_pass123`.

a. Create a new git repository `story_official` under max user.

b. SSH into `storage server` using user `max` and password `Max_pass123` and clone this newly created repository under user `max` home directory i.e `/home/max`.

c. Copy all files from location `/usr/devops` to the repository and commit/push your changes to the `master` branch. The commit message must be `"add stories"` (must be done in single commit).

d. Create a new branch `max_apps` from `master`.

e. Copy a file `story-index-max.txt` from location `/tmp/stories/` to the repository. This file has a typo, which you can fix by changing the word `Mooose` to `Mouse`. Commit and push the changes to the newly created branch. Commit message must be `typo fixed for Mooose` (must be done in single commit).

Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


## Solution:   


**1. This task require  Gitea UI Login**

- Create a new git repository story_blog under max user.

![login page](https://github.com/dineshrajdhanapathyDD/dineshrajdhanapathyDD/assets/52989362/6f0c80e2-7fd4-4b2c-abef-db7593023317)


![create repo](https://github.com/dineshrajdhanapathyDD/dineshrajdhanapathyDD/assets/52989362/fdcaea63-d44c-48a1-b571-4a0101714865)


**2. Login on storage server with user max**

```
thor@jump_host ~$ ls
thor@jump_host ~$ pwd
/home/thor
thor@jump_host ~$ ssh max@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:0z85j/k+4Nf8WKbHJzxo1AOv4FeRA8LPET2N3BEkYyo.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
max@ststor01's password: Max_pass123
Welcome to xFusionCorp Storage server.
max $
``` 


**3. Clone git repo which was created in Gitea UI**


![clone repo ](https://github.com/dineshrajdhanapathyDD/dineshrajdhanapathyDD/assets/52989362/3c80ae73-475f-4df9-921c-74908c20279c)


```
max $ pwd
/home/max
max $ ls
max $ git clone http://git.stratos.xfusioncorp.com/max/story_official.git
Cloning into 'story_official'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
max $ ls
story_official
max $ cd story_official
max $ ls
max $ pwd
/home/max/story_official
```

**4. Copy all files from the location /usr/itadmin to the repository**

```
max $ cp /usr/devops/*.* .
max $ ls
frogs-and-ox.txt    lion-and-mouse.txt
max $ 
```

**5. Git add & Commit with message given in task**

```
max $ git add .
max $ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   frogs-and-ox.txt
        new file:   lion-and-mouse.txt

max $ git commit -m "add stories"
[master (root-commit) 9cf3d43] add stories
 Committer: Linux User <max@ststor01.stratos.xfusioncorp.com>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 2 files changed, 42 insertions(+)
 create mode 100644 frogs-and-ox.txt
 create mode 100644 lion-and-mouse.txt

max (master)$ git log
commit 9cf3d43bb6a3bf143632dee382ade87f6c0671d8
Author: Linux User <max@ststor01.stratos.xfusioncorp.com>
Date:   Fri Oct 13 10:22:54 2023 +0000

    add stories
```

**6. Create a new branch from master as per the task** 

```
max (master)$ git checkout -b max_apps
Switched to a new branch 'max_apps'
```

**7. Copy story-index file from tmp to clone repo directory**

```
max (max_apps)$ cp /tmp/stories/story-index-max.txt .
```

**8. Correct the file & git add , Commit with message given in task** 

- (fix by changing the word Mooose to Mouse).


```
max (max_apps)$ vi story-index-max.txt
max (max_apps)$ cat story-index-max.txt
1. The Lion and the Mooose -> mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dogmax 
(max_apps)$


(max_apps)$ git add .
max (max_apps)$ git status
On branch max_apps
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   story-index-max.txt

max (max_apps)$ git commit -m "typo fixed for Mooose"
[max_apps 4b3e295] typo fixed for Mooose
 Committer: Linux User <max@ststor01.stratos.xfusioncorp.com>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 1 file changed, 4 insertions(+)
 create mode 100644 story-index-max.txt
max (max_apps)$ 

max (max_apps)$ git status
On branch max_apps
nothing to commit, working directory clean
max (max_apps)$ 

max (max_apps)$ git push -u origin master
Username for 'http://git.stratos.xfusioncorp.com': max
Password for 'http://max@git.stratos.xfusioncorp.com': Max_pass123
Counting objects: 4, done.
Delta compression using up to 36 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 1.19 KiB | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
remote: . Processing 1 references
remote: Processed 1 references in total
To http://git.stratos.xfusioncorp.com/max/story_official.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
max (max_apps)$ 
```

![4 story_official ](https://github.com/dineshrajdhanapathyDD/dineshrajdhanapathyDD/assets/52989362/a09846f1-54b6-4715-a0cd-279731845fcb)


```
max (max_apps)$ git log
commit 4b3e295b41211f94bee7e2d79e5738f26fa64ab6
Author: Linux User <max@ststor01.stratos.xfusioncorp.com>
Date:   Fri Oct 13 10:30:25 2023 +0000
    typo fixed for Mooose
commit 9cf3d43bb6a3bf143632dee382ade87f6c0671d8
Author: Linux User <max@ststor01.stratos.xfusioncorp.com>
Date:   Fri Oct 13 10:22:54 2023 +0000
    add stories
max (max_apps)$ 
```

![5 master branch](https://github.com/dineshrajdhanapathyDD/dineshrajdhanapathyDD/assets/52989362/d52e6bb6-6046-4737-8185-1fb847bd5db2)

```
max (max_apps)$ git log --decorate --name-only
commit 4b3e295b41211f94bee7e2d79e5738f26fa64ab6 (HEAD -> max_apps)
Author: Linux User <max@ststor01.stratos.xfusioncorp.com>
Date:   Fri Oct 13 10:30:25 2023 +0000
    typo fixed for Mooose
story-index-max.txt
commit 9cf3d43bb6a3bf143632dee382ade87f6c0671d8 (origin/master, master)
Author: Linux User <max@ststor01.stratos.xfusioncorp.com>
Date:   Fri Oct 13 10:22:54 2023 +0000
    add stories
frogs-and-ox.txt
lion-and-mouse.txt
max (max_apps)$ 

max (max_apps)$ git push -u origin max_apps
Username for 'http://git.stratos.xfusioncorp.com': max
Password for 'http://max@git.stratos.xfusioncorp.com': Max_pass123
Counting objects: 3, done.
Delta compression using up to 36 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 414 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a new pull request for 'max_apps':
remote:   http://git.stratos.xfusioncorp.com/max/story_official/compare/master...max_apps
remote: 
remote: . Processing 1 references
remote: Processed 1 references in total
To http://git.stratos.xfusioncorp.com/max/story_official.git
 * [new branch]      max_apps -> max_apps
Branch max_apps set up to track remote branch max_apps from origin.
max (max_apps)$ 

max (max_apps)$ git log
commit 4b3e295b41211f94bee7e2d79e5738f26fa64ab6
Author: Linux User <max@ststor01.stratos.xfusioncorp.com>
Date:   Fri Oct 13 10:30:25 2023 +0000

    typo fixed for Mooose

commit 9cf3d43bb6a3bf143632dee382ade87f6c0671d8
Author: Linux User <max@ststor01.stratos.xfusioncorp.com>
Date:   Fri Oct 13 10:22:54 2023 +0000
    add stories
max (max_apps)$ 
```

**9. Verify the changes reflect on branch and master.**

![6 max branch add](https://github.com/dineshrajdhanapathyDD/dineshrajdhanapathyDD/assets/52989362/5cf500e1-f27f-4afc-a024-4a0eb0796f2d)

![7 Branch max_apps](https://github.com/dineshrajdhanapathyDD/dineshrajdhanapathyDD/assets/52989362/77eff0a9-708c-4b68-aee6-26f4e1977bec)


**10. Click on Finish & Confirm to complete the task successful**