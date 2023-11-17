

## Questions:

The Nautilus DevOps team wants to install and set up a simple `httpd` web server on all app servers in `Stratos DC`. Additionally, they want to deploy a sample web page for now using Ansible only. Therefore, write the required playbook to complete this task. Find more details about the task below.



We already have an `inventory` file under `/home/thor/ansible` directory on `jump host`. Create a `playbook.yml` under `/home/thor/ansible` directory on `jump host` itself.


Using the playbook, install `httpd` web server on all app servers. Additionally, make sure its service should up and running.


Using `blockinfile` Ansible module add some content in `/var/www/html/index.html` file. Below is the content:


`Welcome to XfusionCorp!`

- This is Nautilus sample file, created using Ansible!

`Please do not modify this file manually!`


The `/var/www/html/index.html` file's user and group `owner` should be `apache` on all app servers.


The `/var/www/html/index.html` file's permissions should be `0777` on all app servers.


`Note`:

i. Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure the playbook works this way without passing any extra arguments.


ii. Do not use any custom or empty `marker` for `blockinfile` module.


## Solution:  

**1. Go through the folder mentioned in task and create inventory & playbook files**    

```

thor@jump_host ~$ cd /home/thor/ansible/
thor@jump_host ~/ansible$ ls
ansible.cfg  inventory
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

     - name: Add content block in index.html and set permissions

       blockinfile:

         path: /var/www/html/index.html

         create: yes

         block: |

           Welcome to XfusionCorp!

           This is Nautilus sample file, created using Ansible!

           Please do not modify this file manually!

         owner: apache

         group: apache

         mode: "0777"
```

**3. Post file saved , run below command to execute the playbook**

```

thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Install httpd and setup index.html] ****************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Install httpd] *************************************************************************************************************************************************************************
changed: [stapp03]
changed: [stapp02]
changed: [stapp01]

TASK [Start service httpd, if not started] ***************************************************************************************************************************************************
changed: [stapp02]
changed: [stapp01]
changed: [stapp03]

TASK [Add content block in index.html and set permissions] ***********************************************************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP ***********************************************************************************************************************************************************************************
stapp01                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

**4. validate the task by curl on app server and check**  

```

thor@jump_host ~/ansible$ ansible all -a 'ls -l /var/www/html/' -i inventory
stapp03 | CHANGED | rc=0 >>
total 4
-rwxrwxrwx 1 apache apache 179 Sep 10 12:40 index.html
stapp01 | CHANGED | rc=0 >>
total 4
-rwxrwxrwx 1 apache apache 179 Sep 10 12:40 index.html
stapp02 | CHANGED | rc=0 >>
total 4
-rwxrwxrwx 1 apache apache 179 Sep 10 12:40 index.html


thor@jump_host ~/ansible$ curl http://stapp01
# BEGIN ANSIBLE MANAGED BLOCK

Welcome to XfusionCorp!

This is Nautilus sample file, created using Ansible!

Please do not modify this file manually!
# END ANSIBLE MANAGED BLOCK


thor@jump_host ~/ansible$ curl http://stapp02
# BEGIN ANSIBLE MANAGED BLOCK

Welcome to XfusionCorp!

This is Nautilus sample file, created using Ansible!

Please do not modify this file manually!
# END ANSIBLE MANAGED BLOCK


thor@jump_host ~/ansible$ curl http://stapp03
# BEGIN ANSIBLE MANAGED BLOCK

Welcome to XfusionCorp!

This is Nautilus sample file, created using Ansible!

Please do not modify this file manually!
# END ANSIBLE MANAGED BLOCK
```

**5. Click on `Finish` & `Confirm` to complete the task successful.**