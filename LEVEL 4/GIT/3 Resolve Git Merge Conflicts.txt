

Questions:

Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details:

SSH into (storage server) using user (max) and password (Max_pass123). Under (/home/max) you will find the (story-blog) repository. Try to push the changes to the origin repo and fix the issues. The (story-index.txt) must have titles for all 4 stories. Additionally, there is a typo in The (Lion and the Mooose) line where (Mooose) should be (Mouse).

Click on the (Gitea UI) button on the top bar. You should be able to access the (Gitea) page. You can login to (Gitea) server from UI using username (sarah) and password (Sarah_pass123) or username (max) and password (Max_pass123).

(Note): For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


Solution:  

Click on the (Gitea UI) button 

Login to (Gitea) server from UI using username  (max) and password (Max_pass123).

![sign in](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/f43262f7-4244-438f-b0e8-008fcca1a2aa)

Able to see the project in the (Gitea) page

![project page](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c173dee1-ef73-4bc2-ab53-7f1b09db0164)

open the folder stroy-index.txt file 

![4](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/087694fe-8784-46fb-a11e-c9f4836d822b)


Terminal 

1. At first login on storage server  using user max given in task    

thor@jump_host ~$ ssh max@ststor01
The authenticity of host 'ststor01 (172.16.238.15)' can't be established.
ECDSA key fingerprint is SHA256:0z85j/k+4Nf8WKbHJzxo1AOv4FeRA8LPET2N3BEkYyo.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ststor01,172.16.238.15' (ECDSA) to the list of known hosts.
max@ststor01's password: 
Welcome to xFusionCorp Storage server.
max $ 


2. Try to git push you will get error failed to push due to not config user

max $ cd /home/max/story-blog/
max (master)$ ls
fox-and-grapes.txt  frogs-and-ox.txt    lion-and-mouse.txt  story-index.txt
max (master)$ cat story-index.txt
1. The Lion and the Mooose
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dogmax (master)$ 

max (master)$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
nothing to commit, working directory clean
max (master)$ 

max (master)$ git push
Username for 'http://git.stratos.xfusioncorp.com': max
Password for 'http://max@git.stratos.xfusioncorp.com': Max_pass123
To http://git.stratos.xfusioncorp.com/sarah/story-blog.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'http://git.stratos.xfusioncorp.com/sarah/story-blog.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
max (master)$ 


3. Add config max user

max (master)$ git config --global --add user.email max@stratos.xfusioncorp.com
max (master)$ git config --global --add user.name max
max (master)$ 


4. Now you can pull the repo

max (master)$ git pull origin master
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From http://git.stratos.xfusioncorp.com/sarah/story-blog
 * branch            master     -> FETCH_HEAD
   6dde876..8219155  master     -> origin/master
Auto-merging story-index.txt
CONFLICT (add/add): Merge conflict in story-index.txt
Automatic merge failed; fix conflicts and then commit the result.
max (master)$ 


5. Edit the merge conflict story file need to do change 

max (master)$ vi story-index.txt
max (master)$ cat story-index.txt
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
max (master)$ 


6. Add and commit the story  file 

max (master)$ git add story-index.txt

max (master)$ git commit -m "fix typo and merge request"
[master d787388] fix typo and merge request

max (master)$ git push origin master
Username for 'http://git.stratos.xfusioncorp.com': max
Password for 'http://max@git.stratos.xfusioncorp.com': Max_pass123
Counting objects: 10, done.
Delta compression using up to 36 threads.
Compressing objects: 100% (10/10), done.
Writing objects: 100% (10/10), 1.37 KiB | 0 bytes/s, done.
Total 10 (delta 3), reused 0 (delta 0)
remote: . Processing 1 references
remote: Processed 1 references in total
To http://git.stratos.xfusioncorp.com/sarah/story-blog.git
   8219155..d787388  master -> master
max (master)$ 

max (master)$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
max (master)$ 

![Fix typo](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9afc1c77-a6a7-4ee4-9fe4-e24f39713048)

max (master)$ git pull origin master
From http://git.stratos.xfusioncorp.com/sarah/story-blog
 * branch            master     -> FETCH_HEAD
Already up-to-date.
max (master)$ 


7. Validate the task by GUI

![6](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e1f4b451-997b-45f5-b16a-b357c539072e)




