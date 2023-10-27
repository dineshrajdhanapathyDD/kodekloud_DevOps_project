



## Questions

Several new developers and DevOps engineers just joined the xFusionCorp industries. They have been assigned the  `Nautilus`  project, and as per the onboarding process we need to create user accounts for new joinees on at least one of the app servers in  `Stratos DC`. We also need to create groups and make new users members of those groups. We need to accomplish this task using Ansible. Below you can find more information about the task.

  

There is already an  `inventory`  file  `~/playbooks/inventory`  on  `jump host`.  
  

On  `jump host`  itself there is a list of users in  `~/playbooks/data/users.yml`  file and there are two groups —  `admins`  and  `developers`  —that have list of different users. Create a playbook  `~/playbooks/add_users.yml`  on  `jump host`  to perform the following tasks on  `app server 2`  in  `Stratos DC`.  
  

a. Add all users given in the  `users.yml`  file on  `app server 2`.  
  

b. Also add  `developers`  and  `admins`  groups on the same server.  
  

c. As per the list given in the  `users.yml`  file, make each user member of the respective group they are listed under.  
  

d. Make sure home directory for all of the users under  `developers`  group is  `/var/www`  (not the default i.e  `/var/www/{USER}`). Users under  `admins`  group should use the default home directory (i.e  `/home/devid`  for user  `devid`).  
  

e. Set password  `YchZHRcLkL`  for all of the users under  `developers`  group and  `8FmzjvFU6S`  for of the users under  `admins`  group. Make sure to use the password given in the  `~/playbooks/secrets/vault.txt`  file as Ansible vault password to encrypt the original password strings. You can use  `~/playbooks/secrets/vault.txt`  file as a vault secret file while running the playbook (make necessary changes in  `~/playbooks/ansible.cfg`  file).  
  

f. All users under  `admins`  group must be added as sudo users. To do so, simply make them member of the  `wheel`  group as well.  
  

`Note:`  Validation will try to run the playbook using command  `ansible-playbook -i inventory add_users.yml`  so please make sure playbook works this way, without passing any extra arguments.



## Solution:

**1. Check inventory, vault & user file as per your task**
```

thor@jump_host ~$ cd ~/playbooks/
thor@jump_host ~/playbooks$ ls
ansible.cfg  data  inventory  secrets

thor@jump_host ~/playbooks$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner

thor@jump_host ~/playbooks$ cat data/users.yml
admins:
  - rob
  - david
  - joy

developers:
  - tim
  - ray
  - jim
  - mark
thor@jump_host ~/playbooks$ cat secrets/vault.txt
P@ss3or432

```

**2. Kindly add the vault in the ansible configuration file**

```

thor@jump_host ~/playbooks$ vi ansible.cfg
thor@jump_host ~/playbooks$ cat ansible.cfg
[defaults]
host_key_checking = False
vault_password_file = /home/thor/playbooks/secrets/vault.txt

```
**3. To check ansible inventory file run command on given app server  in task**

```

thor@jump_host ~/playbooks$ ansible stapp02 -a "cat /etc/passwd" -i inventory
stapp02 | CHANGED | rc=0 >>
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
systemd-resolve:x:193:193:systemd Resolver:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/dev/null:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
ansible:x:1000:1000::/home/ansible:/bin/bash
steve:x:1001:1001::/home/steve:/bin/bash

```
**4. Create the playbook with given name in task**
**(P.S)** - `make sure to change the password as per task`


```

thor@jump_host ~/playbooks$ vi add_users.yml
thor@jump_host ~/playbooks$ cat add_users.yml
---                                                                                                              
- name: Ansbile Add User & Group                                                                       
  hosts: stapp02                                                                                                
  become: yes                                                                                                    
  tasks:                                                                                                         
  - name: Creating Admin Groups                                                                                  
    group:                                                                                                       
     name:                                                                                                       
      admins                                                                                                     
     state: present                                                                                              
  - name: Creating Dev Groups                                                                                    
    group:                                                                                                       
     name:                                                                                                       
      developers                                                                                                 
     state: present                                                                                              
  - name: Creating Admins Group Users                                                                            
    user:                                                                                                        
     name: "{{ item }}"                                                                                          
     password: "{{ '8FmzjvFU6S' | password_hash ('sha512') }}"                                                   
     groups: admins,wheel
     state: present                                                                                              
    loop:                                                                                                        
    - rob                                                                                                        
    - joy                                                                                                        
    - david                                                                                                      
  - name: Creating Developers Group Users                                                                        
    user:                                                                                                        
     name: "{{ item }}"                                                                                          
     password: "{{ 'YchZHRcLkL' | password_hash ('sha512') }}"                                                   
     home: "/var/www/{{ item }}"                                                                                             
     group: developers                                                                                           
     state: present                                                                                              
    loop:                                                                                                        
    - tim                                                                                                        
    - jim                                                                                                        
    - mark                                                                                                       
    - ray  
 
```
**5. Run the playbook using command  `ansible-playbook -i inventory add_users.yml`** 
```

thor@jump_host ~/playbooks$ ansible-playbook -i inventory add_users.yml

PLAY [Ansbile Add User & Group] ********************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [stapp02]

TASK [Creating Admin Groups] ***********************************************************************************************
changed: [stapp02]

TASK [Creating Dev Groups] *************************************************************************************************
changed: [stapp02]

TASK [Creating Admins Group Users] *****************************************************************************************
[DEPRECATION WARNING]: Encryption using the Python crypt module is deprecated. The Python crypt module is deprecated and 
will be removed from Python 3.13. Install the passlib library for continued encryption functionality. This feature will be 
removed in version 2.17. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
changed: [stapp02] => (item=rob)
changed: [stapp02] => (item=joy)
changed: [stapp02] => (item=david)

TASK [Creating Developers Group Users] *************************************************************************************
[DEPRECATION WARNING]: Encryption using the Python crypt module is deprecated. The Python crypt module is deprecated and 
will be removed from Python 3.13. Install the passlib library for continued encryption functionality. This feature will be 
removed in version 2.17. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
changed: [stapp02] => (item=tim)
changed: [stapp02] => (item=jim)
changed: [stapp02] => (item=mark)
changed: [stapp02] => (item=ray)

PLAY RECAP *****************************************************************************************************************
stapp02                    : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```

**6. Validate the task by checking all users created as per task**

```

thor@jump_host ~/playbooks$ ansible stapp02 -a "cat /etc/passwd" -i inventory
stapp02 | CHANGED | rc=0 >>
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
systemd-resolve:x:193:193:systemd Resolver:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/dev/null:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
ansible:x:1000:1000::/home/ansible:/bin/bash
steve:x:1001:1001::/home/steve:/bin/bash
rob:x:1002:1004::/home/rob:/bin/bash
joy:x:1003:1005::/home/joy:/bin/bash
david:x:1004:1006::/home/david:/bin/bash
tim:x:1005:1003::/var/www/tim:/bin/bash
jim:x:1006:1003::/var/www/jim:/bin/bash
mark:x:1007:1003::/var/www/mark:/bin/bash
ray:x:1008:1003::/var/www/ray:/bin/bash

```

```

thor@jump_host ~/playbooks$ ssh rob@stapp02
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:TyLnNwamV6KhjaYyZh0ZS3P8FTN0BpP8Tm1BH5X+iYY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02' (ECDSA) to the list of known hosts.
rob@stapp02's password: 
[rob@stapp02 ~]$ sudo su -
[root@stapp02 ~]# logout
[rob@stapp02 ~]$ id
uid=1002(rob) gid=1004(rob) groups=1004(rob),10(wheel),1002(admins)
[rob@stapp02 ~]$ id joy
uid=1003(joy) gid=1005(joy) groups=1005(joy),10(wheel),1002(admins)
 
```
 **7. Click on `Finish` & Confirm` to complete the task successful**

![ansi 42](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3e0814d7-07d2-44a6-92b2-43f69b57fecd)