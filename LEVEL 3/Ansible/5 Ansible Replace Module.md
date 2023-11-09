

## Questions:

There is some data on all app servers in `Stratos DC`. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes they made. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.


Write a `playbook.yml` under `/home/thor/ansible` on `jump host`, an `inventory` is already present under `/home/thor/ansible` directory on `Jump host` itself. Perform below given tasks using this playbook:


1. We have a file `/opt/finance/blog.txt` on app server 1. Using Ansible `replace` module replace string `xFusionCorp` to `Nautilus` in that file.


2. We have a file `/opt/finance/story.txt` on app server 2. Using Ansible`replace` module replace the string `Nautilus` to `KodeKloud` in that file.


3. We have a file `/opt/finance/media.txt` on app server 3. Using Ansible `replace` module replace string `KodeKloud` to `xFusionCorp Industries` in that file.


`Note`: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.


## Solution:  

**1. Go through the folder mentioned in task and verified the playbook**     

```

thor@jump_host ~$ cd /home/thor/ansible
thor@jump_host ~/ansible$ ls
ansible.cfg  inventory
thor@jump_host ~/ansible$ 
```

**2.  Create a playbook as per the task**

```

thor@jump_host ~/ansible$ vi playbook.yml
thor@jump_host ~/ansible$ cat playbook.yml
- name: Ansible replace

  hosts: stapp01,stapp02,stapp03

  become: yes

  tasks:

    - name: blog.txt replacement

      replace:

        path: /opt/finance/blog.txt

        regexp: "xFusionCorp"

        replace: "Nautilus"

      when: inventory_hostname == "stapp01"

    - name: story.txt replacement

      replace:

        path: /opt/finance/story.txt

        regexp: "Nautilus"

        replace: "KodeKloud"

      when: inventory_hostname == "stapp02"

    - name: media.txt replacement

      replace:

        path: /opt/finance/media.txt

        regexp: "KodeKloud"

        replace: "xFusionCorp Industries"

      when: inventory_hostname == "stapp03"
thor@jump_host ~/ansible$ 
```

**3. Post file saved , run below command to execute the playbook**

```

thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Ansible replace] *****************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp02]
ok: [stapp01]
ok: [stapp03]

TASK [blog.txt replacement] ************************************************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [story.txt replacement] ***********************************************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [media.txt replacement] ***********************************************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   

thor@jump_host ~/ansible$ 
```

**4. Validate the task by login and cat the file**

```

thor@jump_host ~/ansible$ ssh -t tony@stapp01 "cat /opt/finance/blog.txt"
The authenticity of host 'stapp01 (172.16.238.10)' can't be established.
ECDSA key fingerprint is SHA256:lGtwgxAZqqw4FUs/YmqCF68tzwBcw39xiCvrDibZwIs.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01' (ECDSA) to the list of known hosts.
tony@stapp01's password: 
Welcome to Nautilus Industries !
Connection to stapp01 closed.
thor@jump_host ~/ansible$ 
```

**5. Click on `Finish` & `Confirm` to complete the task successful**
