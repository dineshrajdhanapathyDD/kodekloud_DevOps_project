

## Questions:

The Nautilus DevOps team want to install and set up a simple `httpd` web server on all app servers in `Stratos DC`. They also want to deploy a sample web page using Ansible. Therefore, write the required playbook to complete this task as per details mentioned below.


We already have an `inventory` file under `/home/thor/ansible` directory on `jump host`. Write a playbook `playbook.yml` under `/home/thor/ansible` directory on `jump host` itself. Using the playbook perform below given tasks:


1. Install `httpd` web server on all app servers, and make sure its service is up and running.


2. Create a file `/var/www/html/index.html` with content:


(This is a Nautilus sample file, created using Ansible!)


3. Using `lineinfile` Ansible module add some more content in `/var/www/html/index.html` file. Below is the content:

`Welcome to Nautilus Group!`


Also make sure this new line is added at the top of the file.


4. The `/var/www/html/index.html` file's user and group `owner` should be `apache` on all app servers.


5. The `/var/www/html/index.html` file's permissions should be `0655` on all app servers.


`Note`: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.


## Solution:  

**1. Go through the folder mentioned in task and create inventory & playbook files**

```

thor@jump_host ~$ cd   /home/thor/ansible 
thor@jump_host ~/ansible$ ls
ansible.cfg  inventory
thor@jump_host ~/ansible$ 
```

**2. Create a playbook file   as per the task**

```

thor@jump_host ~/ansible$ vi playbook.yml
thor@jump_host ~/ansible$ cat playbook.yml
- name: Install httpd and setup index.html

  hosts: stapp01, stapp02, stapp03

  become: yes

  tasks:

    - name: Install httpd

      package:

        name: httpd

        state: present

    - name: Start service httpd, if not started

      service:

        name: httpd

        state: started

    - name: Add content in index.html. Create file if it does not exist and set file attributes

      copy:

        dest: /var/www/html/index.html

        content: This is a Nautilus sample file, created using Ansible!

        mode: "0655"

        owner: apache

        group: apache

    - name: Update content in index.html

      lineinfile:

        path: /var/www/html/index.html

        insertbefore: BOF

        line: Welcome to xFusionCorp Industries!
thor@jump_host ~/ansible$ 
```

**3. Post file saved , run below command to execute the playbook**

```

thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Install httpd and setup index.html] **********************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp02]
ok: [stapp01]
ok: [stapp03]

TASK [Install httpd] *******************************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

TASK [Start service httpd, if not started] *********************************************************************************
changed: [stapp02]
changed: [stapp03]
changed: [stapp01]

TASK [Add content in index.html. Create file if it does not exist and set file attributes] *********************************
changed: [stapp01]
changed: [stapp03]
changed: [stapp02]

TASK [Update content in index.html] ****************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP *****************************************************************************************************************
stapp01                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

thor@jump_host ~/ansible$ 
```

**4. validate the task by curl on app server and check**

```

thor@jump_host ~/ansible$ ansible all -a "cat /var/www/html/index.html" -i inventory
stapp03 | CHANGED | rc=0 >>
Welcome to xFusionCorp Industries!
This is a Nautilus sample file, created using Ansible!
stapp02 | CHANGED | rc=0 >>
Welcome to xFusionCorp Industries!
This is a Nautilus sample file, created using Ansible!
stapp01 | CHANGED | rc=0 >>
Welcome to xFusionCorp Industries!
This is a Nautilus sample file, created using Ansible!
thor@jump_host ~/ansible$ curl http://stapp01
Welcome to xFusionCorp Industries!
This is a Nautilus sample file, created using Ansible!
thor@jump_host ~/ansible$ curl http://stapp02
Welcome to xFusionCorp Industries!
This is a Nautilus sample file, created using Ansible!
thor@jump_host ~/ansible$ curl http://stapp03
Welcome to xFusionCorp Industries!
This is a Nautilus sample file, created using Ansible!thor@jump_host ~/ansible$ 
```

**5. Click on `Finish` & `Confirm` to complete the task successful**