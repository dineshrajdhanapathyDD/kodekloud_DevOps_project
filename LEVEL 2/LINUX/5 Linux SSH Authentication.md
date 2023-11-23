
## Question:

The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure the thor user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e tony for app server 1). Based on the requirements, perform the following:

Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.

## Solution:  

**1.  1st check your login as thor user  on the server  & create a key by below command**    

```
thor@jump_host /$ whoami


thor@jump_host /$ ssh-keygen -t rsa
```

**2.  Copy public key by below command on all the APP server's**  

```
thor@jump_host /$ ssh-copy-id  tony@stapp01

tony@stapp01's password: Ir0nM@n


thor@jump_host /$ ssh-copy-id  steve@stapp02

steve@stapp02's password: Am3ric@


thor@jump_host /$ ssh-copy-id  banner@stapp03

banner@stapp03's password: BigGr33n

thor@jump_host /$ ssh banner@stapp03
[banner@stapp03 ~]$ logout
connection to stapp03 closed
```


**3.  Now try logging into the APP server without password. in this eg. I am login on stapp01**

```
thor@jump_host /$ ssh tony@stapp01

[tony@stapp01 ~]$ whoami

tony
```