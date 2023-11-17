

## Questions:

The Nautilus DevOps team is planning to test several Ansible playbooks on different app servers in `Stratos DC`. Before that, some pre-requisites must be met. Essentially, the team needs to set up a password-less SSH connection between Ansible controller and Ansible managed nodes. One of the tickets is assigned to you; please complete the task as per details mentioned below:

a. `Jump host` is our Ansible controller, and we are going to run Ansible playbooks through `thor` user from `jump host`.

b. There is an inventory file `/home/thor/ansible/inventory` on `jump host`. Using that inventory file test `Ansible ping` from `jump host` to `App Server 1`, make sure ping works.


## Solution :

**1.  check the ansible inventory file on jump server host**

```

thor@jump_host ~$ cd /home/thor/ansible/
thor@jump_host ~/ansible$ ls
inventory

thor@jump_host ~/ansible$ vi inventory
thor@jump_host ~/ansible$ cat inventory
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

**2. Generate a SSH key on Jump Host . Please note it should Thor user**

```

thor@jump_host ~/ansible$ ssh-keygen -t rsa -b 2048
Generating public/private rsa key pair.
Enter file in which to save the key (/home/thor/.ssh/id_rsa): 
/home/thor/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/thor/.ssh/id_rsa.
Your public key has been saved in /home/thor/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:SgWvEAaujUbTrj3ouAPfhWDX3svhG6B84beVR1/sF7U thor@jump_host.stratos.xfusioncorp.com
The key's randomart image is:
+---[RSA 2048]----+
|  ..o .          |
| ... . o         |
| o....  o       .|
|.+= ...o       .o|
|oo.+ o=.S   .  Eo|
|o +..+o+o  o . o.|
|.+ +o.+oooo . . o|
|+ . o. .++ .    .|
|o+      o.       |
+----[SHA256]-----+
```

**3. Copy SSH key to setup password-less authentication to the host**

```

thor@jump_host ~/ansible$ ssh-copy-id  tony@stapp01
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/thor/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
tony@stapp01's password: Ir0nM@n

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'tony@stapp01'"
and check to make sure that only the key(s) you wanted were added.


thor@jump_host ~/ansible$ ssh tony@stapp01
tony@stapp01's password: 
Last login: Fri Sep  8 13:44:48 2023 from 172.16.238.3
[tony@stapp01 ~]$ exit
logout
Connection to stapp01 closed.
```

**4.  Validate the task by below command**

```

thor@jump_host ~/ansible$ ansible stapp01 -m ping -i inventory -v
Using /etc/ansible/ansible.cfg as config file
stapp01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
thor@jump_host ~/ansible$ 
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**



