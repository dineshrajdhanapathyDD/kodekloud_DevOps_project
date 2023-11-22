

Questions:
To secure our (Nautilus) infrastructure in (Stratos Datacenter) we have decided to install and configure (firewalld) on all app servers. We have Apache and Nginx services running on these apps. Nginx is running as a reverse proxy server for Apache. We might have more robust firewall settings in the future, but for now we have decided to go with the given requirements listed below:

a. Allow all incoming connections on Nginx port, i.e (80).

b. Block all incoming connections on Apache port, i.e (8080).

c. All rules must be permanent.

d. Zone should be public.

e. If Apache or Nginx services aren't running already, please make sure to start them.


Solution:  

1. At first login on App server ssh tony@stapp01

thor@jump_host ~$ ssh tony@stapp01
tony@stapp01's password: Ir0nM@n


2. Switch to root user : sudo su -

[tony@stapp01 ~]$ sudo su -
[sudo] password for tony: Ir0nM@n


3. Run Below command to check the existing Apache httpd & Nginx service status.
[root@stapp01 ~]# systemctl status httpd &&  systemctl status nginx
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Sat 2023-08-26 17:55:19 UTC; 6min ago
     Docs: man:httpd.service(8)
 Main PID: 573 (httpd)
   Status: "Running, listening on: port 8080"
    Tasks: 213 (limit: 1340692)
   Memory: 20.6M
   CGroup: /docker/bd1d697a7193f2052d20a27a05c88e9ce703ae4098ea0fc861be4e807bb3487d/system.slice/httpd.service
           ├─573 /usr/sbin/httpd -DFOREGROUND
           ├─592 /usr/sbin/httpd -DFOREGROUND
           ├─594 /usr/sbin/httpd -DFOREGROUND
           ├─595 /usr/sbin/httpd -DFOREGROUND
           └─596 /usr/sbin/httpd -DFOREGROUND

Aug 26 18:00:39 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:00:49 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:00:59 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:01:09 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:01:19 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:01:29 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:01:39 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:01:49 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:01:59 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
Aug 26 18:02:09 stapp01.stratos.xfusioncorp.com systemd[1]: httpd.service: Got notification message 
from PID 573 (READY=1, STATUS=Running, listening on: port 8080)
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: active (running) since Sat 2023-08-26 17:55:20 UTC; 6min ago
  Process: 851 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  Process: 838 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Process: 837 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
 Main PID: 864 (nginx)
    Tasks: 37 (limit: 1340692)
   Memory: 54.4M
      CGroup: /docker/bd1d697a7193f2052d20a27a05c88e9ce703ae4098ea0fc861be4e807bb3487d/system.slice/nginx.service
           ├─864 nginx: master process /usr/sbin/nginx
           ├─865 nginx: worker process
           ├─866 nginx: worker process
           ├─867 nginx: worker process
           ├─868 nginx: worker process
           ├─869 nginx: worker process
           ├─870 nginx: worker process
           ├─871 nginx: worker process
           ├─872 nginx: worker process
           ├─873 nginx: worker process
           ├─874 nginx: worker process
           ├─875 nginx: worker process
           ├─876 nginx: worker process
           ├─877 nginx: worker process
           ├─878 nginx: worker process
           ├─879 nginx: worker process
           ├─880 nginx: worker process
           ├─881 nginx: worker process
           ├─882 nginx: worker process
           ├─883 nginx: worker process
           ├─884 nginx: worker process
           ├─885 nginx: worker process
           ├─886 nginx: worker process
           ├─887 nginx: worker process
           ├─888 nginx: worker process
           ├─889 nginx: worker process
           ├─890 nginx: worker process
           ├─891 nginx: worker process
           ├─892 nginx: worker process
           ├─893 nginx: worker process
           ├─894 nginx: worker process
           ├─895 nginx: worker process
           ├─896 nginx: worker process
           ├─897 nginx: worker process
           ├─898 nginx: worker process
           ├─899 nginx: worker process
           └─900 nginx: worker process

Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[851]: nginx.service: Executing: /usr/sbin/ng
inx
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Child 851 belongs to ngin
x.service.
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Control process exited, c
ode=exited status=0
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Got final SIGCHLD for sta
te start.
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: New main PID 864 belongs 
to service, we are happy.
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Main PID loaded: 864
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Changed start -> running
0m
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Job nginx.service/start f
inished, result=done
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: Started The nginx HTTP and reverse proxy server.
Aug 26 17:55:20 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Failed to send unit chang
e signal for nginx.service: Connection reset by peer

4. Get the Apache & Nginx Listen port  by using the below command

[root@stapp01 ~]# grep -i Listen /etc/httpd/conf/ht*  /etc/nginx/nginx.conf
/etc/httpd/conf/httpd.conf:# Listen: Allows you to bind Apache to specific IP addresses and/or
/etc/httpd/conf/httpd.conf:# Change this to Listen on specific IP addresses as shown below to 
/etc/httpd/conf/httpd.conf:#Listen 12.34.56.78:80
/etc/httpd/conf/httpd.conf:Listen 8080
/etc/nginx/nginx.conf:        listen       80 default_server;
/etc/nginx/nginx.conf:        listen       [::]:80 default_server;
/etc/nginx/nginx.conf:#        listen       443 ssl http2 default_server;
/etc/nginx/nginx.conf:#        listen       [::]:443 ssl http2 default_server;

5.  Now Install Firewalld : 

[root@stapp01 ~]# yum install -y firewalld
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                                                  15 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                                                  53 MB/s |  33 MB     00:00    
CentOS Stream 8 - BaseOS                                                                     11 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                                    9.8 MB/s |  46 MB     00:04    
CentOS Stream 8 - Extras                                                                    6.9 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                                    4.4 kB/s | 3.0 kB     00:00    
CentOS Stream 8 - Extras common packages                                                     11 kB/s | 6.8 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                                              1.5 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                                           6.9 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                                   356 kB/s |  99 kB     00:00    
Dependencies resolved.
============================================================================================================================
 Package                                Architecture           Version                         Repository              Size
============================================================================================================================
Installing:
 firewalld                              noarch                 0.9.11-1.el8                    baseos                 580 k
Installing dependencies:
 firewalld-filesystem                   noarch                 0.9.11-1.el8                    baseos                  78 k
 ipset                                  x86_64                 7.1-1.el8                       baseos                  45 k
 ipset-libs                             x86_64                 7.1-1.el8                       baseos                  71 k
 iptables                               x86_64                 1.8.5-8.el8                     baseos                 591 k
 iptables-ebtables                      x86_64                 1.8.5-8.el8                     baseos                  74 k
 iptables-libs                          x86_64                 1.8.5-8.el8                     baseos                 103 k
 jansson                                x86_64                 2.14-1.el8                      baseos                  47 k
 libibverbs                             x86_64                 46.0-1.el8.1                    baseos                 410 k
 libmnl                                 x86_64                 1.0.4-6.el8                     baseos                  30 k
 libnetfilter_conntrack                 x86_64                 1.0.6-5.el8                     baseos                  65 k
 libnfnetlink                           x86_64                 1.0.1-13.el8                    baseos                  33 k
 libnftnl                               x86_64                 1.2.2-3.el8                     baseos                  87 k
 libpcap                                x86_64                 14:1.9.1-5.el8                  baseos                 169 k
 nftables                               x86_64                 1:1.0.4-2.el8                   baseos                 380 k
 python3-firewall                       noarch                 0.9.11-1.el8                    baseos                 472 k
 python3-libselinux                     x86_64                 2.9-8.el8                       baseos                 283 k
 python3-nftables                       x86_64                 1:1.0.4-2.el8                   baseos                  31 k
 python3-slip                           noarch                 0.6.4-13.el8                    baseos                  39 k
 python3-slip-dbus                      noarch                 0.6.4-13.el8                    baseos                  39 k

Transaction Summary
============================================================================================================================
Install  20 Packages

Total download size: 3.5 M
Installed size: 10 M
Downloading Packages:
(1/20): ipset-7.1-1.el8.x86_64.rpm                                                          190 kB/s |  45 kB     00:00    
(2/20): firewalld-filesystem-0.9.11-1.el8.noarch.rpm                                        282 kB/s |  78 kB     00:00    
(3/20): ipset-libs-7.1-1.el8.x86_64.rpm                                                     561 kB/s |  71 kB     00:00    
(4/20): iptables-ebtables-1.8.5-8.el8.x86_64.rpm                                            864 kB/s |  74 kB     00:00    
(5/20): iptables-1.8.5-8.el8.x86_64.rpm                                                     2.2 MB/s | 591 kB     00:00    
(6/20): iptables-libs-1.8.5-8.el8.x86_64.rpm                                                1.1 MB/s | 103 kB     00:00    
(7/20): jansson-2.14-1.el8.x86_64.rpm                                                       547 kB/s |  47 kB     00:00    
(8/20): libmnl-1.0.4-6.el8.x86_64.rpm                                                       486 kB/s |  30 kB     00:00    
(9/20): libibverbs-46.0-1.el8.1.x86_64.rpm                                                  2.1 MB/s | 410 kB     00:00    
(10/20): libnetfilter_conntrack-1.0.6-5.el8.x86_64.rpm                                      1.0 MB/s |  65 kB     00:00    
(11/20): libnfnetlink-1.0.1-13.el8.x86_64.rpm                                               524 kB/s |  33 kB     00:00    
(12/20): libnftnl-1.2.2-3.el8.x86_64.rpm                                                    1.3 MB/s |  87 kB     00:00    
(13/20): libpcap-1.9.1-5.el8.x86_64.rpm                                                     2.1 MB/s | 169 kB     00:00    
(14/20): nftables-1.0.4-2.el8.x86_64.rpm                                                    4.2 MB/s | 380 kB     00:00    
(15/20): python3-libselinux-2.9-8.el8.x86_64.rpm                                            3.5 MB/s | 283 kB     00:00    
(16/20): python3-firewall-0.9.11-1.el8.noarch.rpm                                           4.1 MB/s | 472 kB     00:00    
(17/20): python3-nftables-1.0.4-2.el8.x86_64.rpm                                            509 kB/s |  31 kB     00:00    
(18/20): python3-slip-0.6.4-13.el8.noarch.rpm                                               616 kB/s |  39 kB     00:00    
(19/20): python3-slip-dbus-0.6.4-13.el8.noarch.rpm                                          623 kB/s |  39 kB     00:00    
(20/20): firewalld-0.9.11-1.el8.noarch.rpm                                                  399 kB/s | 580 kB     00:01    
----------------------------------------------------------------------------------------------------------------------------
Total                                                                                       2.3 MB/s | 3.5 MB     00:01     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                    1/1 
  Installing       : libmnl-1.0.4-6.el8.x86_64                                                                         1/20 
  Running scriptlet: libmnl-1.0.4-6.el8.x86_64                                                                         1/20 
  Installing       : libnftnl-1.2.2-3.el8.x86_64                                                                       2/20 
  Running scriptlet: libnftnl-1.2.2-3.el8.x86_64                                                                       2/20 
  Installing       : libnfnetlink-1.0.1-13.el8.x86_64                                                                  3/20 
  Running scriptlet: libnfnetlink-1.0.1-13.el8.x86_64                                                                  3/20 
  Installing       : libnetfilter_conntrack-1.0.6-5.el8.x86_64                                                         4/20 
  Running scriptlet: libnetfilter_conntrack-1.0.6-5.el8.x86_64                                                         4/20 
  Installing       : ipset-libs-7.1-1.el8.x86_64                                                                       5/20 
  Running scriptlet: ipset-libs-7.1-1.el8.x86_64                                                                       5/20 
  Installing       : ipset-7.1-1.el8.x86_64                                                                            6/20 
  Installing       : python3-libselinux-2.9-8.el8.x86_64                                                               7/20 
  Installing       : python3-slip-0.6.4-13.el8.noarch                                                                  8/20 
  Installing       : python3-slip-dbus-0.6.4-13.el8.noarch                                                             9/20 
  Installing       : libibverbs-46.0-1.el8.1.x86_64                                                                   10/20 
  Installing       : libpcap-14:1.9.1-5.el8.x86_64                                                                    11/20 
  Installing       : iptables-libs-1.8.5-8.el8.x86_64                                                                 12/20 
  Running scriptlet: iptables-1.8.5-8.el8.x86_64                                                                      13/20 
  Installing       : iptables-1.8.5-8.el8.x86_64                                                                      13/20 
  Running scriptlet: iptables-1.8.5-8.el8.x86_64                                                                      13/20 
  Installing       : iptables-ebtables-1.8.5-8.el8.x86_64                                                             14/20 
  Running scriptlet: iptables-ebtables-1.8.5-8.el8.x86_64                                                             14/20 
  Installing       : jansson-2.14-1.el8.x86_64                                                                        15/20 
  Installing       : nftables-1:1.0.4-2.el8.x86_64                                                                    16/20 
  Running scriptlet: nftables-1:1.0.4-2.el8.x86_64                                                                    16/20 
  Installing       : python3-nftables-1:1.0.4-2.el8.x86_64                                                            17/20 
  Installing       : python3-firewall-0.9.11-1.el8.noarch                                                             18/20 
  Installing       : firewalld-filesystem-0.9.11-1.el8.noarch                                                         19/20 
  Installing       : firewalld-0.9.11-1.el8.noarch                                                                    20/20 
  Running scriptlet: firewalld-0.9.11-1.el8.noarch                                                                    20/20 
  Verifying        : firewalld-0.9.11-1.el8.noarch                                                                     1/20 
  Verifying        : firewalld-filesystem-0.9.11-1.el8.noarch                                                          2/20 
  Verifying        : ipset-7.1-1.el8.x86_64                                                                            3/20 
  Verifying        : ipset-libs-7.1-1.el8.x86_64                                                                       4/20 
  Verifying        : iptables-1.8.5-8.el8.x86_64                                                                       5/20 
  Verifying        : iptables-ebtables-1.8.5-8.el8.x86_64                                                              6/20 
  Verifying        : iptables-libs-1.8.5-8.el8.x86_64                                                                  7/20 
  Verifying        : jansson-2.14-1.el8.x86_64                                                                         8/20 
  Verifying        : libibverbs-46.0-1.el8.1.x86_64                                                                    9/20 
  Verifying        : libmnl-1.0.4-6.el8.x86_64                                                                        10/20 
  Verifying        : libnetfilter_conntrack-1.0.6-5.el8.x86_64                                                        11/20 
  Verifying        : libnfnetlink-1.0.1-13.el8.x86_64                                                                 12/20 
  Verifying        : libnftnl-1.2.2-3.el8.x86_64                                                                      13/20 
  Verifying        : libpcap-14:1.9.1-5.el8.x86_64                                                                    14/20 
  Verifying        : nftables-1:1.0.4-2.el8.x86_64                                                                    15/20 
  Verifying        : python3-firewall-0.9.11-1.el8.noarch                                                             16/20 
  Verifying        : python3-libselinux-2.9-8.el8.x86_64                                                              17/20 
  Verifying        : python3-nftables-1:1.0.4-2.el8.x86_64                                                            18/20 
  Verifying        : python3-slip-0.6.4-13.el8.noarch                                                                 19/20 
  Verifying        : python3-slip-dbus-0.6.4-13.el8.noarch                                                            20/20 
Installed products updated.

Installed:
  firewalld-0.9.11-1.el8.noarch          firewalld-filesystem-0.9.11-1.el8.noarch    ipset-7.1-1.el8.x86_64                 
  ipset-libs-7.1-1.el8.x86_64            iptables-1.8.5-8.el8.x86_64                 iptables-ebtables-1.8.5-8.el8.x86_64   
  iptables-libs-1.8.5-8.el8.x86_64       jansson-2.14-1.el8.x86_64                   libibverbs-46.0-1.el8.1.x86_64         
  libmnl-1.0.4-6.el8.x86_64              libnetfilter_conntrack-1.0.6-5.el8.x86_64   libnfnetlink-1.0.1-13.el8.x86_64       
  libnftnl-1.2.2-3.el8.x86_64            libpcap-14:1.9.1-5.el8.x86_64               nftables-1:1.0.4-2.el8.x86_64          
  python3-firewall-0.9.11-1.el8.noarch   python3-libselinux-2.9-8.el8.x86_64         python3-nftables-1:1.0.4-2.el8.x86_64  
  python3-slip-0.6.4-13.el8.noarch       python3-slip-dbus-0.6.4-13.el8.noarch      

Complete!


6. Start the firewalld service , Enable and Check the status 

[root@stapp01 ~]# systemctl start firewalld && systemctl enable firewalld && systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: active (running) since Sat 2023-08-26 18:07:07 UTC; 582ms ago
     Docs: man:firewalld(1)
 Main PID: 1425 (firewalld)
    Tasks: 1 (limit: 1340692)
   Memory: 38.0M
   CGroup: /docker/bd1d697a7193f2052d20a27a05c88e9ce703ae4098ea0fc861be4e807bb3487d/system.slice/firewalld.service
           └─1425 /usr/libexec/platform-python -s /usr/sbin/firewalld --nofork --nopid

Aug 26 18:07:07 stapp01.stratos.xfusioncorp.com systemd[1]: Starting firewalld - dynamic firewall daemon...
Aug 26 18:07:07 stapp01.stratos.xfusioncorp.com systemd[1425]: firewalld.service: Executing: /usr/sb
in/firewalld --nofork --nopid
Aug 26 18:07:07 stapp01.stratos.xfusioncorp.com systemd[1]: firewalld.service: D-Bus name org.fedora
project.FirewallD1 now registered by :1.47
Aug 26 18:07:07 stapp01.stratos.xfusioncorp.com systemd[1]: firewalld.service: Changed start -> runn
ing
Aug 26 18:07:07 stapp01.stratos.xfusioncorp.com systemd[1]: firewalld.service: Job firewalld.service
/start finished, result=done
Aug 26 18:07:07 stapp01.stratos.xfusioncorp.com systemd[1]: Started firewalld - dynamic firewall daemon.
Aug 26 18:07:07 stapp01.stratos.xfusioncorp.com systemd[1]: firewalld.service: Failed to send unit c
hange signal for firewalld.service: Connection reset by peer
Aug 26 18:07:07 stapp01.stratos.xfusioncorp.com firewalld[1425]: WARNING: AllowZoneDrifting is enabl
ed. This is considered an insecure configuration option. It will be removed in a future release. Please consider disabling i
t now.
Aug 26 18:07:08 stapp01.stratos.xfusioncorp.com systemd[1]: firewalld.service: Changed dead -> runni
ng
Aug 26 18:07:08 stapp01.stratos.xfusioncorp.com systemd[1]: firewalld.service: Failed to set invocat
ion ID on control group /docker/bd1d697a7193f2052d20a27a05c88e9ce703ae4098ea0fc861be4e807bb3487d/system.slice/firewalld.serv
ice, ignoring: Operation not permitted


7. Allow the nginx port

( Please make sure, you use  nginx port, refer Point 4. Above)

[root@stapp01 ~]# firewall-cmd --permanent --zone=public --add-port=8096/tcp
success


8. Allow services http & https port 

[root@stapp01 ~]# firewall-cmd --permanent --zone=public --add-service={http,https}
success


9. Allow the Apache http  port  with LB host IP

[root@stapp01 ~]# firewall-cmd --permanent --zone=public --add-rich-rule='rule family="ipv4" source                 address=172.16.238.14 port protocol=tcp port=8083 accept'
success


10. Reload firewalld service to take effect & validate the rules


Please Note :- I have shown only for stapp01. 
You have to do this in all app server 
stapp01: 
server name-stapp01
User -  	
tony

Password
Ir0nM@n

stapp02: server name-stapp02
User -  	
steve

Password
Am3ric@


stapp03: server name-stapp03

User -  	
banner

Password
BigGr33n


11.  Click on Finish & Confirm to complete the task successfully





