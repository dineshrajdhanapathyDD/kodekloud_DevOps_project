

## Questions:
A new teammate has joined the Nautilus application development team, the application development team has asked the DevOps team to create a new user account for the new teammate on application `server 2` in Stratos Datacenter. The task needs to be performed using Puppet only. You can find more details below about the task.



Create a Puppet programming file official.pp under `/etc/puppetlabs/code/environments/production/manifests` directory on master node i.e Jump Server, and using Puppet user resource add a user on all app servers as mentioned below:

Create a user mariyam and set its UID to `1061` on Puppet agent nodes 2 i.e `App Servers 2`.

Notes: 
- Please make sure to run the puppet agent test using sudo on agent nodes, otherwise you can face certificate issues. In that case you will have to clean the certificates first and then you will be able to run the puppet agent test.

- Before clicking on the Check button please make sure to verify puppet server and puppet agent services are up and running on the respective servers, also please make sure to run puppet agent test to apply/test the changes manually first.

- Please note that once lab is loaded, the puppet server service should start automatically on puppet master server, however it can take upto 2-3 minutes to start.


## solutions:

**1. Switch to  root user** 

```
thor@jump_host ~$ sudo su -
[sudo] password for thor: mjolnir123


root@jump_host ~# cd /etc/puppetlabs/code/environments/production/manifests
root@jump_host /etc/puppetlabs/code/environments/production/manifests# ll
total 0
root@jump_host /etc/puppetlabs/code/environments/production/manifests# vi official.pp
open terminal to paste the 
root@jump_host /etc/puppetlabs/code/environments/production/manifests# cat official.pp
class user_creator {

  user { 'mariyam':

    ensure   => present,

    uid => 1061,

  }

}

node 'stapp01.stratos.xfusioncorp.com', 'stapp02.stratos.xfusioncorp.com', 'stapp03.stratos.xfusioncorp.com' {

  include user_creator

}
```

**2. Validate the puppet files by below command.**

```
root@jump_host /etc/puppetlabs/code/environments/production/manifests# puppet parser validate official.pp
```

**3. Login on all  App server**

```
thor@jump_host ~$ ssh steve@stapp02
The authenticity of host 'stapp02 (172.16.238.11)' can't be established.
ECDSA key fingerprint is SHA256:QsfBXIQQDT5Xi69yYY4flEmEOkqHDxo/CfLq6yR31WU.
ECDSA key fingerprint is MD5:09:af:72:41:a6:be:b7:62:0f:7c:c7:ab:36:df:44:7f.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'stapp02,172.16.238.11' (ECDSA) to the list of known hosts.
steve@stapp02's password: Am3ric@
[steve@stapp02 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1 Respect the privacy of others.
    #2 Think before you type.
    #3 With great power comes great responsibility.

[sudo] password for steve: Am3ric@

login app server and run below command to pull the configurations:
[root@stapp02 ~]# cat /etc/passwd |grep mariyam
```

**4. Run Puppet agent to pull the configuration from puppet server**

```
[root@stapp02 ~]# puppet agent -tv
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Retrieving locales
Info: Caching catalog for stapp02.stratos.xfusioncorp.com
Info: Applying configuration version '1689441477'
Notice: /Stage[main]/User_creator/User[mariyam]/ensure: created
Notice: Applied catalog in 0.19 seconds
```

**5. Validate the task by running**

```
[root@stapp02 ~]# cat /etc/passwd |grep mariyam
mariyam:x:1061:1061::/home/mariyam:/bin/bash
```







