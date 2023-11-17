

## Questions:

The Nautilus Application development team wanted to test some applications on app servers in `Stratos Datacenter`. They shared some pre-requisites with the DevOps team, and packages need to be installed on app servers. Since we are already using Ansible for automating such tasks, please perform this task using Ansible as per details mentioned below:



1. Create an inventory file `/home/thor/playbook/inventory` on `jump host` and add all app servers in it.


2. Create an Ansible playbook `/home/thor/playbook/playbook.yml` to install `samba` package on `all app servers` using Ansible `yum` module.


3. Make sure user `thor` should be able to run the playbook on `jump host`.

Note: Validation will try to run playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.


## Solution : 

**1.  check the ansible inventory file on jump server host**

```

thor@jump_host ~$ cd /home/thor/playbook/
thor@jump_host ~/playbook$ ls
inventory
thor@jump_host ~/playbook$ vi inventory
thor@jump_host ~/playbook$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

**2. Create an Ansible playbook `/home/thor/playbook/playbook.yml` to install `samba` package on `all app servers` using Ansible `yum` module.**

```

thor@jump_host ~/playbook$ vi playbook.yml
thor@jump_host ~/playbook$ cat playbook.yml
- name: install package
  hosts: all
  gather_facts: false
  become: true
  tasks:
    - yum:
        name: samba
        state: present
```

**3. run playbook using command**

```

thor@jump_host ~/playbook$ ansible-playbook -i inventory playbook.yml

PLAY [install package] ***********************************************************************************************************************************************************************

TASK [yum] ***********************************************************************************************************************************************************************************
changed: [stapp01]
changed: [stapp03]
changed: [stapp02]

PLAY RECAP ***********************************************************************************************************************************************************************************
stapp01                    : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

**4.  Click on `Finish` & `Confirm` to complete the task successful**