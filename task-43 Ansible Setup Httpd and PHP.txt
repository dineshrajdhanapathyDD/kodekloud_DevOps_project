

Questions:
Nautilus Application development team wants to test the Apache and PHP setup on one of the app servers in Stratos Datacenter. They want the DevOps team to prepare an Ansible playbook to accomplish this task. Below you can find more details about the task.



There is an inventory file ~/playbooks/inventory on jump host.

Create a playbook ~/playbooks/httpd.yml on jump host and perform the following tasks on App Server 2.

a. Install httpd and php packages (whatever default version is available in yum repo).

b. Change default document root of Apache to /var/www/html/myroot in default Apache config /etc/httpd/conf/httpd.conf. Make sure /var/www/html/myroot path exists (if not please create the same).

c. There is a template ~/playbooks/templates/phpinfo.php.j2 on jump host. Copy this template to the Apache document root you created as phpinfo.php file and make sure user owner and the group owner for this file is apache user.

d. Start and enable httpd service.

Note: Validation will try to run the playbook using command ansible-playbook -i inventory httpd.yml, so please make sure the playbook works this way without passing any extra arguments.


Solution:  
1. Go through the folder mentioned in task and create inventory & playbook files  

thor@jump_host ~$ cd  ~/playbooks
thor@jump_host ~/playbooks$ ll
total 12
-rw-r--r-- 1 thor thor   36 Jul  8 11:21 ansible.cfg
-rw-r--r-- 1 thor thor  237 Jul  8 11:21 inventory
drwxr-xr-x 2 thor thor 4096 Jul  8 11:21 templates


2. Create a playbook file  httpd.yml as per the task
thor@jump_host ~/playbooks$ vi httpd.yml

open the new terminal, paste the below file 
- name: Setup Httpd and PHP

  hosts: stapp02

  become: yes

  tasks:

    - name: Install latest version of httpd and php

      package:

        name:

          - httpd

          - php

        state: latest

    - name: Replace default DocumentRoot in httpd.conf

      replace:

        path: /etc/httpd/conf/httpd.conf

        regexp: DocumentRoot \"\/var\/www\/html\"

        replace: DocumentRoot "/var/www/html/myroot"

    - name: Create the new DocumentRoot directory if it does not exist

      file:

        path: /var/www/html/myroot

        state: directory

        owner: apache

        group: apache

    - name: Use Jinja2 template to generate phpinfo.php

      template:

        src: /home/thor/playbooks/templates/phpinfo.php.j2

        dest: /var/www/html/myroot/phpinfo.php

        owner: apache

        group: apache

    - name: Start and enable service httpd

      service:

        name: httpd

        state: started

        enabled: yes 

exit control+c :wq

thor@jump_host ~/playbooks$ cat httpd.yml

3. Post file saved , run below command to execute the playbook

thor@jump_host ~/playbooks$ ansible-playbook  -i inventory httpd.yml


4. validate the task by login on app server and check
thor@jump_host ~/playbooks$ ssh steve@stapp02
steve@stapp02's password: Am3ric@

[steve@stapp02 ~]$ rpm -qa |grep httpd
httpd-tools-2.4.6-99.el7.centos.1.x86_64
httpd-2.4.6-99.el7.centos.1.x86_64

[steve@stapp02 ~]$ rpm -qa |grep php
php-cli-5.4.16-48.el7.x86_64
php-common-5.4.16-48.el7.x86_64
php-5.4.16-48.el7.x86_64

5. Click on Finish & Confirm to complete the task successful


