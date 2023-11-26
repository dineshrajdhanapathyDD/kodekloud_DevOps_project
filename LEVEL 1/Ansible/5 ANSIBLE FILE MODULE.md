

## Questions:

The Nautilus DevOps team is working to test several Ansible modules on servers in `Stratos DC`. Recently they wanted to test the file creation on remote hosts using Ansible. Find below more details about the task:

a. Create an inventory file `~/playbook/inventory` on `jump host` and add `all app servers` in it.

b. Create a playbook `~/playbook/playbook.yml` to create a blank file `/opt/code.txt` on all app servers.

c. The `/opt/code.txt` file permission must be `0744`.

d. The user/group owner of file `/opt/code.txt` must be `tony` on `app server 1`, `steve` on `app server 2` and `banner` on `app server 3`.

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml, so please make sure the playbook works this way without passing any extra arguments.


## Solution: 

**1. Go through the folder mentioned in task and verified the playbook** 

```
thor@jump_host ~$ ls -l /home/thor/playbook/
total 0
thor@jump_host ~$ cd ~/playbook
thor@jump_host ~/playbook$ ll
total 0
```

**2.  Create a inventory file and add the app server's as per task.**

```
thor@jump_host ~/playbook$ vi inventory
thor@jump_host ~/playbook$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n: ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner

thor@jump_host ~/playbook$ ll
total 4
-rw-rw-r-- 1 thor thor 241 Aug 12 09:45 inventory
```

**3.  Create a playbook as per the task**

```
thor@jump_host ~/playbook$ vi playbook.yml
thor@jump_host ~/playbook$ cat playbook.yml
- name: Create file in appservers

  hosts: stapp01, stapp02, stapp03

  become: yes

  tasks:

    - name: Create the file and set properties

      file:

        path: /opt/code.txt

        owner: "{{ ansible_user }}"

        group: "{{ ansible_user }}"

        mode: "0744"

        state: touch
```

**4.Post file saved , run below command to execute the playbook**

```
thor@jump_host ~/playbook$ ansible-playbook  -i inventory playbook.yml

PLAY [Create file in appservers] *******************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp01]
ok: [stapp03]
ok: [stapp02]

TASK [Create the file and set properties] **********************************************************************************
changed: [stapp01]
changed: [stapp03]
changed: [stapp02]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

**6. validate the task by running the below command**

```
thor@jump_host ~/playbook$ ansible all -a "ls -lsd /opt/code.txt" -i inventory
stapp03 | CHANGED | rc=0 >>
0 -rwxr--r-- 1 banner banner 0 Aug 12 09:53 /opt/code.txt
stapp01 | CHANGED | rc=0 >>
0 -rwxr--r-- 1 tony tony 0 Aug 12 09:53 /opt/code.txt
stapp02 | CHANGED | rc=0 >>
0 -rwxr--r-- 1 steve steve 0 Aug 12 09:53 /opt/code.txt
```

**7. Click on `Finish` & `Confirm` to complete the task successful**




















