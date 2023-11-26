

## Questions:
here is data on `jump host` that needs to be copied on `all application servers` in `Stratos DC`. Nautilus DevOps team want to perform this task using `Ansible`. Perform the task as per details mentioned below:

a. On `jump host` create an inventory file `/home/thor/ansible/inventory` and add all application servers as managed nodes.

b. On `jump host` create a playbook `/home/thor/ansible/playbook.yml` to copy `/usr/src/dba/index.html` file to all application servers at location `/opt/dba`.

Note: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.


## Solution:  

**1. Go through the folder mentioned in the task and create inventory & playbook files** 

```
thor@jump_host ~$ cd  /home/thor/ansible/
thor@jump_host ~/ansible$ ll
total 0
```

**2.  Create an inventory file and add the app servers as per task.**

```
thor@jump_host ~/ansible$ vi inventory
thor@jump_host ~/ansible$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n  ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@  ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n  ansible_user=banner
```

**3.  Check the inventory file is working correctly by listing folder on all the app servers**

```
thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/dba" -i inventory
stapp03 | CHANGED | rc=0 >>
total 0
stapp01 | CHANGED | rc=0 >>
total 0
stapp02 | CHANGED | rc=0 >>
total 0
```

**4. Create a playbook as per the task** 

```
thor@jump_host ~/ansible$ vi playbook.yml
thor@jump_host ~/ansible$ cat playbook.yml
- name: Ansible copy
  hosts: all
  become: yes
  tasks:
    - name: copy index.html to dba folder
      copy: src=/usr/src/dba/index.html dest=/opt/dba
```

**5.Post file saved, run below command to execute the playbook** 

```
thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Ansible copy] ********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp02]
ok: [stapp01]
ok: [stapp03]

TASK [copy index.html to dba folder] ***************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

thor@jump_host ~/ansible$
```

**6. validate the task by running the below command**

```
thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/dba" -i inventory
stapp01 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 root root 35 Aug 11 12:07 index.html
stapp02 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 root root 35 Aug 11 12:07 index.html
stapp03 | CHANGED | rc=0 >>
total 4
-rw-r--r-- 1 root root 35 Aug 11 12:07 index.html
```

**7. Click on `Finish` & `Confirm` to complete the task successful.**




when i was working face error:
1.If yaml file not correct shown below error:


ERROR! We were unable to read either as JSON nor YAML, these are the errors we got from each:
JSON: No JSON object could be decoded

Syntax Error while loading YAML.
  mapping values are not allowed in this context

The error appears to be in '/home/thor/ansible/playbook.yml': line 7, column 7, but may
be elsewhere in the file depending on the exact syntax problem. 


2. application servers at location /opt/dba not correct :
ls: cannot access '/opt/': No such file or directorynon-zero return code