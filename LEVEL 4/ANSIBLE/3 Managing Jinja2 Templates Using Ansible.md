



## Question

One of the Nautilus DevOps team members is working on to develop a role for  `httpd`  installation and configuration. Work is almost completed, however there is a requirement to add a jinja2 template for  `index.html`  file. Additionally, the relevant task needs to be added inside the role. The inventory file  `~/ansible/inventory`  is already present on  `jump host`  that can be used. Complete the task as per details mentioned below:

  

a. Update  `~/ansible/playbook.yml`  playbook to run the  `httpd`  role on  `App Server 3`.  
  

b. Create a jinja2 template  `index.html.j2`  under  `/home/thor/ansible/role/httpd/templates/`  directory and add a line  `This file was created using Ansible on <respective server>`  (for example  `This file was created using Ansible on stapp01`  in case of  `App Server 1`). Also please make sure not to hard code the server name inside the template. Instead, use  `inventory_hostname`  variable to fetch the correct value.  
  

c. Add a task inside  `/home/thor/ansible/role/httpd/tasks/main.yml`  to copy this template on  `App Server 3`  under  `/var/www/html/index.html`. Also make sure that  `/var/www/html/index.html`  file's permissions are  `0744`.  
  

d. The user/group owner of  `/var/www/html/index.html`  file must be respective sudo user of the server (for example  `tony`  in case of  `stapp01`).  
  

`Note:`  Validation will try to run the playbook using command  `ansible-playbook -i inventory playbook.yml`  so please make sure the playbook works this way without passing any extra arguments.

## Solution


**1. Go through the folder mentioned in task and verified the playbook**

```

thor@jump_host ~$ cd  ~/ansible/
thor@jump_host ~/ansible$ ls
inventory  playbook.yml  role

```

**2. Edit the exiting file `/home/thor/ansible/playbook.yml` and add the required host**

```

thor@jump_host ~/ansible$ cat playbook.yml
---
- hosts: 
  become: yes
  become_user: root
  roles:
    - role/httpdthor@jump_host ~/ansible$ 
thor@jump_host ~/ansible$ vi playbook.yml
thor@jump_host ~/ansible$ cat playbook.yml
---
- hosts: stapp03
  become: yes
  become_user: root
  roles:
    - role/httpd

```

**3. Create a Jinja2 template file with the below content**

```

thor@jump_host ~/ansible$ vi /home/thor/ansible/role/httpd/templates/index.html.j2
thor@jump_host ~/ansible$ cat /home/thor/ansible/role/httpd/templates/index.html.j2
This file was created using Ansible on {{ ansible_hostname }}

```


**4. Edit the file to add a task to copy the Jinja2 template**

```

thor@jump_host ~/ansible$ vi /home/thor/ansible/role/httpd/tasks/main.yml
thor@jump_host ~/ansible$ cat /home/thor/ansible/role/httpd/tasks/main.yml
---
# tasks file for role/test

- name: install the latest version of HTTPD
  yum:
    name: httpd
    state: latest

- name: Start service httpd
  service:
    name: httpd
    state: started

- name: Use Jinja2 template to generate index.html

  template:

    src: /home/thor/ansible/role/httpd/templates/index.html.j2

    dest: /var/www/html/index.html

    mode: "0744"

    owner: "{{ ansible_user }}"

    group: "{{ ansible_user }}"
thor@jump_host ~/ansible$ 

```


**5. Post file saved , run below command to execute the playbook**

```

thor@jump_host ~/ansible$ ansible-playbook  -i inventory playbook.yml

PLAY [stapp03] *************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp03]

TASK [role/httpd : install the latest version of HTTPD] ********************************************************************
changed: [stapp03]

TASK [role/httpd : Start service httpd] ************************************************************************************
changed: [stapp03]

TASK [role/httpd : Use Jinja2 template to generate index.html] *************************************************************
changed: [stapp03]

PLAY RECAP *****************************************************************************************************************
stapp03                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
 
```
**6. Validate the task by running the below command**

```

thor@jump_host ~/ansible$ ansible stapp03 -a "cat /var/www/html/index.html" -i inventory
stapp03 | CHANGED | rc=0 >>
This file was created using Ansible on stapp03

```

**7. Click on `Finish` & `Confirm` to complete the task successfully**

![ans](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9a3a2a4b-583c-43d8-9e00-9464493afa14)