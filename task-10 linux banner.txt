


Question :  
During the monthly compliance meeting, it was pointed out that several servers in the Stratos DC do not have a valid banner. The security team has provided serveral approved templates which should be applied to the servers to maintain compliance. These will be displayed to the user upon a successful login.


Update the message of the day on all application and db servers for Nautilus. Make use of the approved template located at /tmp/nautilus_banner on jump host



terminal -1

1.  Copy the /tmp/nautilus_banner using scp command from jumpserver to  

all Apps & DB servers.

thor@jump_host /$ ll /tmp/nautilus_banner

thor@jump_host /$ scp -r  /home/thor/nautilus_banner  tony@stapp01:/tmp

tony@stapp01's password: Ir0nM@n

2. Login to all the App server  & switch to root user 

    Now move copied file banner  /tmp to /etc/motd 

thor@jump_host /$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n

[tony@stapp01 ~]$ sudo su -
 [sudo] password for tony:  Ir0nM@n

 [root@stapp01 ~]# mv /tmp/nautilus_banner  /etc/motd

3. Validate the open new terminal and login with user 
thor@jump_host /$ ssh tony@stapp01
 
logout

-
opent next terminal-2


1.  Copy the /tmp/nautilus_banner using scp command from jumpserver to  

all Apps & DB servers.

thor@jump_host /$ ll /tmp/nautilus_banner

thor@jump_host /$ scp -r  /home/thor/nautilus_banner  steve@stapp02:/tmp

steve@stapp02's password: Am3ric@

2. Login to all the App server  & switch to root user 

    Now move copied file banner  /tmp to /etc/motd 

thor@jump_host /$ ssh steve@stapp02
steve@stapp02's password: Am3ric@

[steve@stapp02 ~]$ sudo su -
 [sudo] password for tony:  Am3ric@

 [root@stapp02 ~]# mv /tmp/nautilus_banner  /etc/motd

3. Validate the open new terminal and login with user 
thor@jump_host /$ ssh steve@stapp02
 
logout

-
 open next terminal -3

1.  Copy the /tmp/nautilus_banner using scp command from jumpserver to  

all Apps & DB servers.

thor@jump_host /$ ll /tmp/nautilus_banner

thor@jump_host /$ scp -r  /home/thor/nautilus_banner  banner@stapp03:/tmp

banner@stapp03's password: BigGr33n

2. Login to all the App server  & switch to root user 

    Now move copied file banner  /tmp to /etc/motd 

thor@jump_host /$ ssh steve@stapp02
banner@stapp03's password: BigGr33n

[banner@stapp03 ~]$ sudo su -
 [sudo] password for banner:  BigGr33n

 [root@stapp03 ~]# mv /tmp/nautilus_banner  /etc/motd

3. Validate the open new terminal and login with user 
thor@jump_host /$ ssh banner@stapp03
 
logout


--

next terminal -4


While copying file on DB server you will faced issue due to openssh-clients package not installed.

1. Login on DB server  &  Install openssh-clients

thor@jump_host /$ ssh peter@stdb01


thor@jump_host /$ cat /tmp/p

peter   Sp!dy


open next terminal -5
[peter@stdb01 -]$ sudo yom install openssh-clients

password - Sp!dy


install everything


after install terminal - 4

2.  Copy the /tmp/nautilus_banner using scp command from jumpserver 

thor@jump_host /$ scp -r  /tmp/nautilus_banner  peter@stdb01:/tmp


thor@jump_host /$ ssh -t peter@stdb01 'sudo mv /tmp/nautilus_banner /etc/motd/'



 4. Validate  login all the server's & check banner implemented successfully as per the task request

thor@jump_host /$ ssh peter@stdb01

password : Sp!dy


execute succesful
