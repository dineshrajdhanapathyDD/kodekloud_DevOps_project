


## Question

The Nautilus DevOps team had a discussion about, how they can train different team members to use Ansible for different automation tasks. There are numerous ways to perform a particular task using Ansible, but we want to utilize each aspect that Ansible offers. The team wants to utilise Ansible's conditionals to perform the following task:  
  

An  `inventory`  file is already placed under  `/home/thor/ansible`  directory on  `jump host`, with all the  `Stratos DC app servers`  included.  
  

Create a playbook  `/home/thor/ansible/playbook.yml`  and make sure to use Ansible's  `when`  conditionals statements to perform the below given tasks.

  

1.  Copy  `blog.txt`  file present under  `/usr/src/itadmin`  directory on  `jump host`  to  `App Server 1`  under  `/opt/itadmin`  directory. Its user and group owner must be user  `tony`  and its permissions must be  `0644`  .  
      
    
2.  Copy  `story.txt`  file present under  `/usr/src/itadmin`  directory on  `jump host`  to  `App Server 2`  under  `/opt/itadmin`  directory. Its user and group owner must be user  `steve`  and its permissions must be  `0644`  .  
      
    
3.  Copy  `media.txt`  file present under  `/usr/src/itadmin`  directory on  `jump host`  to  `App Server 3`  under  `/opt/itadmin`  directory. Its user and group owner must be user  `banner`  and its permissions must be  `0644`.  
      
    

`NOTE:`  You can use  `ansible_nodename`  variable from gathered facts with  `when`  condition. Additionally, please make sure you are running the play for all hosts i.e use  `- hosts: all`.  
  

`Note:`  Validation will try to run the playbook using command  `ansible-playbook -i inventory playbook.yml`, so please make sure the playbook works this way without passing any extra arguments.

## Solution

**1. At first ansible inventory file is working properly and doesn't have any file exist from jump server to all the app server's**

```

thor@jump_host ~$ cd /home/thor/ansible/
thor@jump_host ~/ansible$ ls
ansible.cfg  inventory

thor@jump_host ~/ansible$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner

thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/itadmin" -i inventory
stapp01 | CHANGED | rc=0 >>
total 0
stapp03 | CHANGED | rc=0 >>
total 0
stapp02 | CHANGED | rc=0 >>
total 0

thor@jump_host ~/ansible$ ls /usr/src/itadmin
blog.txt  media.txt  story.txt

```

**2. If you see above output then create playbook file**

```

thor@jump_host ~/ansible$ vi playbook.yml
thor@jump_host ~/ansible$ cat playbook.yml
- name: Copy text files to Appservers

  hosts: all

  become: yes

  tasks:

    - name: Copy blog.txt to stapp01

      ansible.builtin.copy:

        src: /usr/src/itadmin/blog.txt

        dest: /opt/itadmin/

        owner: tony

        group: tony

        mode: "0644"

      when: inventory_hostname == "stapp01"

    - name: Copy story.txt to stapp02

      ansible.builtin.copy:

        src: /usr/src/itadmin/story.txt

        dest: /opt/itadmin/

        owner: steve

        group: steve

        mode: "0644"

      when: inventory_hostname == "stapp02"

    - name: Copy media.txt to stapp03

      ansible.builtin.copy:

        src: /usr/src/itadmin/media.txt

        dest: /opt/itadmin/

        owner: banner

        group: banner

        mode: "0644"

      when: inventory_hostname == "stapp03"
 
```

**3. Post file saved , run below command to execute the playbook**


```

thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Copy text files to Appservers] ***************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp03]
ok: [stapp01]
ok: [stapp02]

TASK [Copy blog.txt to stapp01] ********************************************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Copy story.txt to stapp02] *******************************************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Copy media.txt to stapp03] *******************************************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0 

```
**4. Validate the task by below command**

```

thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/itadmin" -i inventory
stapp02 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 steve steve 27 Oct 27 14:50 story.txt
stapp03 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 banner banner 22 Oct 27 14:50 media.txt
stapp01 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 tony tony 35 Oct 27 14:50 blog.txt

```


 **5. Click on `Finish` & `Confirm` to complete the task successful**


![ans 45](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e6bccd1b-f8cc-4d68-ad79-9d06e7606797)