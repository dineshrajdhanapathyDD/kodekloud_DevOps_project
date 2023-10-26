

## Question

The Nautilus DevOps team is trying to setup a simple Apache web server on all app servers in Stratos DC using Ansible. They also want to create a sample html page for now with some app specific data on it. Below you can find more details about the task.

  

You will find a valid inventory file  `/home/thor/playbooks/inventory`  on jump host (which we are using as an Ansible controller).  
  

1.  Create a playbook  `index.yml`  under  `/home/thor/playbooks`  directory on  `jump host`. Using  `blockinfile`  Ansible module create a file  `facts.txt`  under  `/root`  directory on all app servers and add the following given block in it. You will need to enable facts gathering for this task.  
      
    

```
Ansible managed node architecture is <architecture>
```

  
  
(You can obtain the system architecture from Ansible's gathered facts by using the correct Ansible variable while taking into account Jinja2 syntax)  
  

2.  Install  `httpd`  server on all apps. After that make a copy of  `facts.txt`  file as  `index.html`  under  `/var/www/html`  directory. Make sure to start  `httpd`  service after that.  
      
    

`Note:`  Do not create a separate role for this task, just add all of the changes in  `index.yml`  playbook.


## Solution:

1. Go through the folder mentioned in task and create inventory & playbook files

**cd /home/thor/playbooks/**

2. Create a **playbook** file as per the task

```
thor@jump_host ~$ curl http://stapp01
curl: (7) Failed to connect to stapp01 port 80: Connection refused
```

```thor@jump_host ~$ cd /home/thor/playbooks/inventory
bash: cd: /home/thor/playbooks/inventory: Not a directory
```
```
thor@jump_host ~$ cd /home/thor/playbooks/
thor@jump_host ~/playbooks$ ls
ansible.cfg  inventory
thor@jump_host ~/playbooks$ ll
bash: ll: command not found
thor@jump_host ~/playbooks$ ls -l
total 8
-rw-r--r-- 1 thor thor  36 Oct 25 11:32 ansible.cfg
-rw-r--r-- 1 thor thor 237 Oct 25 11:32 inventory

```

- Create a playbook  `index.yml`
```
thor@jump_host ~/playbooks$ vi index.yml
thor@jump_host ~/playbooks$ cat index.yml
---

-

  hosts: stapp01, stapp02, stapp03

  gather_facts: true

  become: yes

  become_method: sudo

  tasks:

    - name: create a  file using blockinfile

      blockinfile:

       create: yes

       path: /root/facts.txt

       block: |

         Ansible managed node architecture is {{ ansible_architecture }}

    - name: Install apache packages

      package:

       name: httpd

    - name: file copy

      shell: cp /root/facts.txt /var/www/html/index.html

    - name: ensure httpd is running

      systemd:

       name: httpd

       state: restarted
thor@jump_host ~/playbooks$ 
```

3.  Post file saved , run below command to execute the playbook

**ansible-playbook -i inventory index.yml**


```
thor@jump_host ~/playbooks$ ansible-playbook  -i inventory index.yml

PLAY [stapp01, stapp02, stapp03] *******************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp03]
ok: [stapp02]
ok: [stapp01]

TASK [create a  file using blockinfile] ************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Install apache packages] *********************************************************************************************
changed: [stapp02]
changed: [stapp03]
changed: [stapp01]

TASK [file copy] ***********************************************************************************************************
changed: [stapp03]
changed: [stapp02]
changed: [stapp01]

TASK [ensure httpd is running] *********************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

thor@jump_host ~/playbooks$ 
```

4. validate the task by curl to all app server and check

**curl http://stapp01**
**curl http://stapp02**
**curl http://stapp03**


```
thor@jump_host ~/playbooks$ curl http://stapp01
# BEGIN ANSIBLE MANAGED BLOCK

Ansible managed node architecture is x86_64
# END ANSIBLE MANAGED BLOCK
thor@jump_host ~/playbooks$ curl http://stapp02
# BEGIN ANSIBLE MANAGED BLOCK

Ansible managed node architecture is x86_64
# END ANSIBLE MANAGED BLOCK
thor@jump_host ~/playbooks$ curl http://stapp03
# BEGIN ANSIBLE MANAGED BLOCK

Ansible managed node architecture is x86_64
# END ANSIBLE MANAGED BLOCK
thor@jump_host ~/playbooks$ 
```

**5. Click on `Finish` & `Confirm` to complete the task successful**
![ansi 41](https://github.com/joseeden/KodeKloud_Engineer_Labs/assets/52989362/7da36f98-5d6d-41c2-9f03-d629bd171399)
