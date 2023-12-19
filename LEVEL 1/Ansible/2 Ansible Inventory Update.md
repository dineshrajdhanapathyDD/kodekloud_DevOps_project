

## Questions:

The Nautilus DevOps team has started testing their Ansible playbooks on different servers within the stack. They have placed some playbooks under `/home/thor/playbook/` directory on jump host which they want to test. Some of these playbooks have already been tested on different servers, but now they want to test them on `app server 3` in Stratos DC. However, they first need to create an inventory file so that Ansible can connect to the respective app. Below are some requirements:



a. Create an ini type Ansible inventory file `/home/thor/playbook/inventory` on jump host.

b. Add `App Server 3` in this inventory along with required variables that are needed to make it work.

c. The inventory hostname of the host should be the server name as per the wiki, for example stapp01 for app server 1 in Stratos DC.

Note: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.


## Solution: 

**1. Go through the folder mentioned in the task and verified the playbook and files existing**

```
thor@jump_host ~$ ls -l /home/thor/playbook/
total 8
-rw-r--r-- 1 thor thor  36 Jul 19 12:48 ansible.cfg
-rw-r--r-- 1 thor thor 250 Jul 19 12:48 playbook.yml

thor@jump_host ~$ cat  /home/thor/playbook/playbook.yml
---
- hosts: all
  become: yes
  become_user: root
  tasks:
    - name: Install httpd package    
      yum: 
        name: httpd 
        state: installed
    
    - name: Start service httpd
      service:
        name: httpd
        state: startedthor@jump_host ~$
```

**2.  Create an inventory file and add the app server as per your task.**

```
thor@jump_host ~$ vi /home/thor/playbook/inventory
thor@jump_host ~$ cat /home/thor/playbook/inventory
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n  ansible_user=banner
```

**3.  Post file saved, run below command to execute the playbook**

```
thor@jump_host ~/playbook$ ansible-playbook -i inventory playbook.yml

PLAY [all] ****************************************************************************

TASK [Gathering Facts] ****************************************************************
ok: [stapp03]

TASK [Install httpd package] **********************************************************
changed: [stapp03]

TASK [Start service httpd] ************************************************************
changed: [stapp03]

PLAY RECAP ****************************************************************************
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

**4.  Click on `Finish` & `Confirm` to complete the task successfully**
