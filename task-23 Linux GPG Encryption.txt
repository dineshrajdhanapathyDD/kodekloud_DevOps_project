
Questions:
We have confidential data that needs to be transferred to a remote location, so we need to encrypt that data.We also need to decrypt data we received from a remote location in order to understand its content.



On storage server in Stratos Datacenter we have private and public keys stored /home/*_key.asc. Use those keys to perform the following actions.

Encrypt /home/encrypt_me.txt to /home/encrypted_me.asc.

Decrypt /home/decrypt_me.asc to /home/decrypted_me.txt. (Passphrase for decryption and encryption is kodekloud).

note:
The information you find here shows below in tasks to perform with GPG usage for encryption & decryption of files in Linux.

GPG, or GnuPG, stands for GNU Privacy Guard. The GPG Project provides the tools and libraries to allows users to interface with a GUI or command line to integrate encryption with emails and operating systems like Linux.
GPG includes the tools you need to use public-key encryption and digital signatures on your Linux system.


Solution:  
1. Login on storage server & switch to root user 


thor@jump_host ~$ ssh natasha@ststor01

natasha@ststor01's password: Bl@kW

[natasha@ststor01 ~]$ sudo su -

[sudo] password for natasha: Bl@kW

2.  All files located in /home 


[root@ststor01 ~]# cd /home/
[root@ststor01 home]# ll
total 24
drwx------ 1 ansible ansible 4096 Oct 15  2019 ansible
-rw-r--r-- 1 root    root     155 Jun 12 15:34 decrypt_me.asc
-rw-r--r-- 1 root    root      99 Jun 12 15:38 encrypt_me.txt
drwx------ 1 natasha natasha 4096 Jan 12  2020 natasha
-rw-r--r-- 1 root    root    3589 Jun 12 15:38 private_key.asc
-rw-r--r-- 1 root    root    1722 Jun 12 15:38 public_key.asc

3.  Import gpg Private & Public key 


[root@ststor01 home]# gpg --import public_key.asc

gpg: key CCE3AF51: public key "kodekloud <kodekloud@kodekloud.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)


[root@ststor01 home]# gpg --import private_key.asc

gpg: key CCE3AF51: secret key imported
gpg: key CCE3AF51: "kodekloud <kodekloud@kodekloud.com>" not changed
gpg: Total number processed: 1
gpg:              unchanged: 1
gpg:       secret keys read: 1
gpg:   secret keys imported: 1

4. Kindly verify keys are imported successfully

[root@ststor01 home]# gpg --list-keys

[root@ststor01 home]# gpg --list-secret-keys


 5. Will encrypt the file txt in to asc in same folder path 


[root@ststor01 home]# gpg --encrypt -r kodekloud@kodekloud.com --armor < encrypt_me.txt  -o encrypted_me.asc

6. Will decrypt the file asc in to txt  using passphrase within same folder path 


[root@ststor01 home]# gpg --decrypt decrypt_me.asc > decrypted_me.txt

enter passpharse : kodekloud


7. check the encrpyted & decrypted files  in  /home


[root@ststor01 home]# ll
total 32
drwx------ 1 ansible ansible 4096 Oct 15  2019 ansible
-rw-r--r-- 1 root    root      80 Jun 12 15:56 decrypted_me.txt
-rw-r--r-- 1 root    root     155 Jun 12 15:34 decrypt_me.asc
-rw-r--r-- 1 root    root     669 Jun 12 15:53 encrypted_me.asc
-rw-r--r-- 1 root    root      99 Jun 12 15:38 encrypt_me.txt
drwx------ 1 natasha natasha 4096 Jan 12  2020 natasha
-rw-r--r-- 1 root    root    3589 Jun 12 15:38 private_key.asc
-rw-r--r-- 1 root    root    1722 Jun 12 15:38 public_key.asc

8.  Validate the task by cat the files output


 [root@ststor01 home]# cat decrypted_me.txt

Welcome to xFusionCorp Industries. This is KodeKloud System Administration Lab 
[root@ststor01 home]# cat decrypt_me.asc


[root@ststor01 home]# cat encrypt_me.txt
My name is "My Name"

[root@ststor01 home]# cat encrypted_me.asc
-----BEGIN PGP MESSAGE-----
Version: GnuPG v2.0.22 (GNU/Linux)


-----END PGP MESSAGE-----



9. Click on Finish & Confirm to complete the task successfully

