

Questions:
xFusionCorp Industries has planned to set up a common email server in Stork DC. After several meetings and recommendations they have decided to use postfix as their mail transfer agent and dovecot as an IMAP/POP3 server. We would like you to perform the following steps:



Install and configure postfix on Stork DC mail server.

Create an email account siva@stratos.xfusioncorp.com identified by YchZHRcLkL.

Set its mail directory to /home/siva/Maildir.

Install and configure dovecot on the same server.

thor@jump_host ~$ ssh groot@stmail01
groot@stmail01's password: Gr00T123

[groot@stmail01 ~]$ sudo su -
[sudo] password for groot: Gr00T123

[root@stmail01 ~]# rpm -qa |grep pastfix
[root@stmail01 ~]# rpm -qa |grep dovecot


2. Install the packages  Postfix & Dovecot 

[root@stmail01 ~]# yum install postfix -y


[root@stmail01 ~]# yum install dovecot -y(optional) below we add again


[root@stmail01 ~]# vi /etc/postfix/main.cf

open another terminal

going to my hotsname remove # then enter :set nu -> number shown

#myhostname = virtual.domain.tld
edit like this
==>myhostname = stmail01.stratos.xfusioncorp.com

then move to mydomain remove #

#mydomain = domain.tld
edit like this
mydomain = stratos.xfusioncorp.com

then #myorigin = $mydomain
remove # edit like this
myorigin = $mydomain

then
#inet_interfaces = all
    133 #inet_interfaces = $myhostname
    134 #inet_interfaces = $myhostname, localhost
    135 inet_interfaces = localhost

first line # remove and last line insert #

inet_interfaces = all
    133 #inet_interfaces = $myhostname
    134 #inet_interfaces = $myhostname, localhost
    135 #inet_interfaces = localhost

next step
183 mydestination = $myhostname, localhost.$mydomain, localhost
    184 #mydestination = $myhostname, localhost.$mydomain, localhost, $mydomai
edit like this
==>insert # first line , remove# second line
183 #mydestination = $myhostname, localhost.$mydomain, localhost
    184 mydestination = $myhostname, localhost.$mydomain, localhost, $mydomai

then move on
283 #mynetworks = 168.100.189.0/28, 127.0.0.0/8
remove # edit like this
283 mynetworks = 172.16.238.0/24, 127.0.0.0/8

last step
438 #home_mailbox = Maildir/
remove #
home_mailbox = Maildir/

control+c
exit use :wq

[root@stmail01 ~]# systemctl start postfix
[root@stmail01 ~]# systemctl status postfix

create user password
[root@stmail01 ~]# useradd siva
[root@stmail01 ~]# passwd siva
Changing password for user siva.
New password: YchZHRcLkL
Retype new password:  YchZHRcLkL
passwd: all authentication tokens updated successfully.

[root@stmail01 ~]# cat /etc/passwd |grep siva
siva:x:1002:1002::/home/siva:/bin/bash

[root@stmail01 ~]# telnet stmail01 25

type:EHLO localhost
shown local host
250-stmail01.stratos.xfusioncorp.com
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-STARTTLS
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250 SMTPUTF8

mail from:siva@stratos.xfusioncorp.com
mail from:kirsty@stratos.xfusioncorp.com

250 2.1.0 Ok

rcpt to:siva@stratos.xfusioncorp.com
rcpt to:kirsty@stratos.xfusioncorp.com
250 2.1.5 Ok

DATA
354 End data with <CR><LF>.<CR><LF>
test mail
.
250 2.0.0 Ok: queued as 7A8D0403D431(88A65579C076)
quit
221 2.0.0 Bye
Connection closed by foreign host.


[root@stmail01 ~]# mailq
Mail queue is empty

[root@stmail01 ~]# su - siva

[siva@stmail01 ~]$ cd Maildir/

cat new/20230626151739.3725E17610A5@stmail01.stratos.xfusioncorp.com

[siva@stmail01 ~]$ exit
logout
exit from user to switch root & install dovecot 

[root@stmail01 ~]$ yum install dovecot -y

lets start with configurations as shown ahead


[root@stmail01 ~]$ vi /etc/dovecot/dovecot.conf

open another terminal 
:set nu

protocols remove #

exit :wq

[root@stmail01 ~]$ vi /etc/dovecot/

[root@stmail01 ~]$ vi /etc/dovecot/conf.d/10-mail.conf
 open another terminal

:set nu
mail_location # remove
mail_location = maildir:~/Maildir
:wq

[root@stmail01 ~]$ vi /etc/dovecot/conf.d/10-auth.conf
 open new terminal
:set nu
remove # 
disable_plaintext_auth = yes
:100
remove # 
auth_mechanisms =  plain add word "login"

exit :wq

[root@stmail01 ~]$ vi /etc/dovecot/conf.d/10-master.conf
open new terminal 
:set nu
:91
 remove #
user = postfix
group = postfix
exit :wq

[root@stmail01 ~]$ systemctl start dovecot
[root@stmail01 ~]$ systemctl status dovecot
active shown

validate the dovecot with telnet
[root@stmail01 ~]$ telnet stmail01 110

user siva

pass 

retr 1
+OK 518 octets
Return-Path: <kirsty@stratos.xfusioncorp.com>
X-Original-To: kirsty@stratos.xfusioncorp.com
Delivered-To: kirsty@stratos.xfusioncorp.com
Received: from localhost (stmail01 [172.16.238.17])
        by stmail01.stratos.xfusioncorp.com (Postfix) with ESMTP id 3725E17610A5
        for <kirsty@stratos.xfusioncorp.com>; Mon, 26 Jun 2023 15:17:21 +0000 (UTC)
Message-Id: <20230626151739.3725E17610A5@stmail01.stratos.xfusioncorp.com>
Date: Mon, 26 Jun 2023 15:17:21 +0000 (UTC)
From: kirsty@stratos.xfusioncorp.com
quit

4. Click on Finish & Confirm to complete the task successfully
