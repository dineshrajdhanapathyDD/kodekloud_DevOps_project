

## Questions:

One of the DevOps team members has created an ZIP archive on jump host in Stratos DC that needs to be extracted and copied over to all app servers in Stratos DC itself. Because this is a routine task, the Nautilus DevOps team has suggested automating it. We can use Ansible since we have been using it for other automation tasks. Below you can find more details about the task:

We have an inventory file under `/home/thor/ansible` directory on jump host, which should have all the app servers added already.

There is a ZIP archive `/usr/src/devops/devops.zip` on jump host.

Create a `playbook.yml` under `/home/thor/ansible/` directory on jump host itself to perform the below given tasks.



Unzip `/usr/src/devops/devops.zip` archive in `/opt/devops/` location on all app servers.

Make sure the extracted data must has the respective sudo user as their user and group owner, i.e `tony` for `app server 1`, `steve` for `app server 2`, `banner` for `app server 3`.

The extracted data permissions must be `0744`

Note: Validation will try to run the playbook using command `ansible-playbook -i inventory playbook.yml` so please make sure playbook works this way, without passing any extra arguments.


## Solution:

**1. Go through the folder mentioned in task and create inventory & playbook files**

```

thor@jump_host /$ cd  /home/thor/ansible/

thor@jump_host ~/ansible$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

**2. Check the existing file & verify the inventory file**

```

thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/devops/" -i inventory
stapp02 | CHANGED | rc=0 >>
total 0
stapp03 | CHANGED | rc=0 >>
total 0
stapp01 | CHANGED | rc=0 >>
total 0

thor@jump_host ~/ansible$ ll /usr/src/devops/
total 4
-rw-r--r-- 1 root root 367 Jul 18 09:59 devops.zip
```

**3.  Create a playbook as per the task**

```

thor@jump_host ~/ansible$ vi playbook.yml
open terminal edit it as per the task
- name: Extract archive
  hosts: stapp01, stapp02, stapp03
  become: yes
  tasks:
    - name: Extract the archive and set the owner/permissions
      unarchive:
        src: /usr/src/devops/devops.zip
        dest: /opt/devops/
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: "0744"

exit :wq
check the vi editor file are correct execute the below command and check:
thor@jump_host ~/ansible$ cat playbook.yml
```

**4. Post file saved , run below command to execute the playbook**

```

thor@jump_host ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Extract archive] ****************************************************************

TASK [Gathering Facts] ****************************************************************
ok: [stapp02]
ok: [stapp01]
ok: [stapp03]

TASK [Extract the archive and set the owner/permissions] ******************************
changed: [stapp02]
changed: [stapp01]
changed: [stapp03]

PLAY RECAP ****************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

**5. validate the task by running the below command**

```

thor@jump_host ~/ansible$ ansible all -a "ls -ltr /opt/devops/" -i inventory
stapp03 | CHANGED | rc=0 >>
total 4
drwxr--r-- 2 banner banner 4096 Jul 18 09:59 unarchive
stapp02 | CHANGED | rc=0 >>
total 4
drwxr--r-- 2 steve steve 4096 Jul 18 09:59 unarchive
stapp01 | CHANGED | rc=0 >>
total 4
drwxr--r-- 2 tony tony 4096 Jul 18 09:59 unarchive
```

**6. Click on `Finish` & `Confirm` to complete the task successful**