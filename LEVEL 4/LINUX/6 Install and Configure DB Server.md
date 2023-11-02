

## Question

We need to setup a database server on  `Nautilus DB Server`  in  `Stratos Datacenter`. Please perform the below given steps on DB Server:  
  
a. Install/Configure MariaDB server.  
  
b. Create a database named  `kodekloud_db3`.  

c. Create a user called  `kodekloud_rin`  and set its password to  `LQfKeWWxWD`.  

d. Grant full permissions to user  `kodekloud_rin`  on database  `kodekloud_db3`.

## Solution

**1. At first login on database server `ssh peter@stdb01`**
```

thor@jump_host ~$ ssh peter@stdb01
The authenticity of host 'stdb01 (172.16.239.10)' can't be established.
ECDSA key fingerprint is SHA256:bwIM5cf3kc6Hxp5a+YsRpL1SuvmlH4EHtMESQUSDksg.
Are you sure you want to continue connecting (yes/no/[fingerprint])? es
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added 'stdb01,172.16.239.10' (ECDSA) to the list of known hosts.
peter@stdb01's password: Sp!dy
```
**2. Switch to root user: `Sudo su -`**
```
[peter@stdb01 ~]$ sudo su -
We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:
    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.
[sudo] password for peter: Sp!dy

```
**3. Install openssh-clients** 
```
[root@stdb01 ~]# yum install openssh-clients -y
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                               30 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                               28 MB/s |  34 MB     00:01    
CentOS Stream 8 - BaseOS                                                  33 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                  54 MB/s |  52 MB     00:00    
CentOS Stream 8 - Extras                                                  16 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                  17 kB/s | 3.0 kB     00:00    
CentOS Stream 8 - Extras common packages                                  28 kB/s | 6.9 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                           1.1 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                         15 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                745 kB/s |  99 kB     00:00    
Package openssh-clients-8.0p1-17.el8.x86_64 is already installed.
Dependencies resolved.
=========================================================================================================
 Package                   Architecture     Version                    Repository                   Size
=========================================================================================================
Upgrading:
 openssh                   x86_64           8.0p1-19.el8_8             ubi-8-baseos-rpms           524 k
 openssh-clients           x86_64           8.0p1-19.el8_8             ubi-8-baseos-rpms           669 k
 openssh-server            x86_64           8.0p1-19.el8_8             ubi-8-baseos-rpms           493 k

Transaction Summary
=========================================================================================================
Upgrade  3 Packages

Total download size: 1.6 M
Downloading Packages:
(1/3): openssh-8.0p1-19.el8_8.x86_64.rpm                                 4.6 MB/s | 524 kB     00:00    
(2/3): openssh-server-8.0p1-19.el8_8.x86_64.rpm                          4.0 MB/s | 493 kB     00:00    
(3/3): openssh-clients-8.0p1-19.el8_8.x86_64.rpm                         5.0 MB/s | 669 kB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                     12 MB/s | 1.6 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Running scriptlet: openssh-8.0p1-19.el8_8.x86_64                                                   1/1 
  Running scriptlet: openssh-8.0p1-19.el8_8.x86_64                                                   1/6 
  Upgrading        : openssh-8.0p1-19.el8_8.x86_64                                                   1/6 
  Upgrading        : openssh-clients-8.0p1-19.el8_8.x86_64                                           2/6 
  Running scriptlet: openssh-server-8.0p1-19.el8_8.x86_64                                            3/6 
  Upgrading        : openssh-server-8.0p1-19.el8_8.x86_64                                            3/6 
  Running scriptlet: openssh-server-8.0p1-19.el8_8.x86_64                                            3/6 
  Running scriptlet: openssh-server-8.0p1-17.el8.x86_64                                              4/6 
  Cleanup          : openssh-server-8.0p1-17.el8.x86_64                                              4/6 
  Running scriptlet: openssh-server-8.0p1-17.el8.x86_64                                              4/6 
  Cleanup          : openssh-clients-8.0p1-17.el8.x86_64                                             5/6 
  Cleanup          : openssh-8.0p1-17.el8.x86_64                                                     6/6 
  Running scriptlet: openssh-8.0p1-17.el8.x86_64                                                     6/6 
  Verifying        : openssh-8.0p1-19.el8_8.x86_64                                                   1/6 
  Verifying        : openssh-8.0p1-17.el8.x86_64                                                     2/6 
  Verifying        : openssh-clients-8.0p1-19.el8_8.x86_64                                           3/6 
  Verifying        : openssh-clients-8.0p1-17.el8.x86_64                                             4/6 
  Verifying        : openssh-server-8.0p1-19.el8_8.x86_64                                            5/6 
  Verifying        : openssh-server-8.0p1-17.el8.x86_64                                              6/6 
Installed products updated.

Upgraded:
  openssh-8.0p1-19.el8_8.x86_64                      openssh-clients-8.0p1-19.el8_8.x86_64              
  openssh-server-8.0p1-19.el8_8.x86_64              

Complete!
```
**4. Install `mariadb-server`**
```

[root@stdb01 ~]# sudo yum install -y mariadb-server
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 0:02:13 ago on Wed Nov  1 18:20:10 2023.
Dependencies resolved.
=========================================================================================================
 Package                       Arch      Version                                      Repository    Size
=========================================================================================================
Installing:
 mariadb-server                x86_64    3:10.3.28-1.module_el8.3.0+757+d382997d      appstream     16 M
Installing dependencies:
 groff-base                    x86_64    1.22.3-18.el8                                baseos       1.0 M
 iproute                       x86_64    6.2.0-5.el8                                  baseos       854 k
 libaio                        x86_64    0.3.112-1.el8                                baseos        33 k
 libbpf                        x86_64    0.5.0-1.el8                                  baseos       137 k
 libmnl                        x86_64    1.0.4-6.el8                                  baseos        30 k
 mariadb                       x86_64    3:10.3.28-1.module_el8.3.0+757+d382997d      appstream    6.0 M
 mariadb-common                x86_64    3:10.3.28-1.module_el8.3.0+757+d382997d      appstream     64 k
 mariadb-connector-c           x86_64    3.1.11-2.el8_3                               appstream    200 k
 mariadb-connector-c-config    noarch    3.1.11-2.el8_3                               appstream     15 k
 mariadb-errmsg                x86_64    3:10.3.28-1.module_el8.3.0+757+d382997d      appstream    234 k
 ncurses                       x86_64    6.1-9.20180224.el8                           baseos       387 k
 perl-Carp                     noarch    1.42-396.el8                                 baseos        30 k
 perl-DBD-MySQL                x86_64    4.046-3.module_el8+353+7103df35              appstream    157 k
 perl-DBI                      x86_64    1.641-4.module_el8+332+132e4365              appstream    765 k
 perl-Data-Dumper              x86_64    2.167-399.el8                                baseos        58 k
 perl-Encode                   x86_64    4:2.97-3.el8                                 baseos       1.5 M
 perl-Errno                    x86_64    1.28-422.el8                                 baseos        76 k
 perl-Exporter                 noarch    5.72-396.el8                                 baseos        34 k
 perl-File-Path                noarch    2.15-2.el8                                   baseos        38 k
 perl-File-Temp                noarch    0.230.600-1.el8                              baseos        63 k
 perl-Getopt-Long              noarch    1:2.50-4.el8                                 baseos        63 k
 perl-HTTP-Tiny                noarch    0.074-1.el8                                  baseos        58 k
 perl-IO                       x86_64    1.38-422.el8                                 baseos       142 k
 perl-MIME-Base64              x86_64    3.15-396.el8                                 baseos        31 k
 perl-Math-BigInt              noarch    1:1.9998.11-7.el8                            baseos       196 k
 perl-Math-Complex             noarch    1.59-422.el8                                 baseos       109 k
 perl-PathTools                x86_64    3.74-1.el8                                   baseos        90 k
 perl-Pod-Escapes              noarch    1:1.07-395.el8                               baseos        20 k
 perl-Pod-Perldoc              noarch    3.28-396.el8                                 baseos        86 k
 perl-Pod-Simple               noarch    1:3.35-395.el8                               baseos       213 k
 perl-Pod-Usage                noarch    4:1.69-395.el8                               baseos        34 k
 perl-Scalar-List-Utils        x86_64    3:1.49-2.el8                                 baseos        68 k
 perl-Socket                   x86_64    4:2.027-3.el8                                baseos        59 k
 perl-Storable                 x86_64    1:3.11-3.el8                                 baseos        98 k
 perl-Term-ANSIColor           noarch    4.06-396.el8                                 baseos        46 k
 perl-Term-Cap                 noarch    1.17-395.el8                                 baseos        23 k
 perl-Text-ParseWords          noarch    3.30-395.el8                                 baseos        18 k
 perl-Text-Tabs+Wrap           noarch    2013.0523-395.el8                            baseos        24 k
 perl-Time-Local               noarch    1:1.280-1.el8                                baseos        34 k
 perl-Unicode-Normalize        x86_64    1.25-396.el8                                 baseos        82 k
 perl-constant                 noarch    1.33-396.el8                                 baseos        25 k
 perl-interpreter              x86_64    4:5.26.3-422.el8                             baseos       6.3 M
 perl-libs                     x86_64    4:5.26.3-422.el8                             baseos       1.6 M
 perl-macros                   x86_64    4:5.26.3-422.el8                             baseos        73 k
 perl-parent                   noarch    1:0.237-1.el8                                baseos        20 k
 perl-podlators                noarch    4.11-1.el8                                   baseos       118 k
 perl-threads                  x86_64    1:2.21-2.el8                                 baseos        61 k
 perl-threads-shared           x86_64    1.58-2.el8                                   baseos        48 k
 psmisc                        x86_64    23.1-5.el8                                   baseos       151 k
Installing weak dependencies:
 mariadb-backup                x86_64    3:10.3.28-1.module_el8.3.0+757+d382997d      appstream    6.1 M
 mariadb-gssapi-server         x86_64    3:10.3.28-1.module_el8.3.0+757+d382997d      appstream     51 k
 mariadb-server-utils          x86_64    3:10.3.28-1.module_el8.3.0+757+d382997d      appstream    1.1 M
 perl-IO-Socket-IP             noarch    0.39-5.el8                                   appstream     47 k
 perl-Mozilla-CA               noarch    20160104-7.module_el8+645+9d809f8c           appstream     15 k
Enabling module streams:
 mariadb                                 10.3                                                           
 perl                                    5.26                                                           
 perl-DBD-MySQL                          4.046                                                          
 perl-DBI                                1.641                                                          
 perl-IO-Socket-SSL                      2.066                                                          
 perl-libwww-perl                        6.34                                                           

Transaction Summary
=========================================================================================================
Install  55 Packages

Total download size: 45 M
Installed size: 197 M
Downloading Packages:
(1/55): mariadb-common-10.3.28-1.module_el8.3.0+757+d382997d.x86_64.rpm  415 kB/s |  64 kB     00:00    
(2/55): mariadb-connector-c-3.1.11-2.el8_3.x86_64.rpm                    2.1 MB/s | 200 kB     00:00    
(3/55): mariadb-connector-c-config-3.1.11-2.el8_3.noarch.rpm             394 kB/s |  15 kB     00:00    
(4/55): mariadb-errmsg-10.3.28-1.module_el8.3.0+757+d382997d.x86_64.rpm  3.3 MB/s | 234 kB     00:00    
(5/55): mariadb-backup-10.3.28-1.module_el8.3.0+757+d382997d.x86_64.rpm   15 MB/s | 6.1 MB     00:00    
(6/55): mariadb-10.3.28-1.module_el8.3.0+757+d382997d.x86_64.rpm          14 MB/s | 6.0 MB     00:00    
(7/55): mariadb-gssapi-server-10.3.28-1.module_el8.3.0+757+d382997d.x86_ 609 kB/s |  51 kB     00:00    
(8/55): perl-DBD-MySQL-4.046-3.module_el8+353+7103df35.x86_64.rpm        3.0 MB/s | 157 kB     00:00    
(9/55): mariadb-server-utils-10.3.28-1.module_el8.3.0+757+d382997d.x86_6  18 MB/s | 1.1 MB     00:00    
(10/55): perl-IO-Socket-IP-0.39-5.el8.noarch.rpm                         1.4 MB/s |  47 kB     00:00    
(11/55): perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch.rpm   441 kB/s |  15 kB     00:00    
(12/55): perl-DBI-1.641-4.module_el8+332+132e4365.x86_64.rpm             7.2 MB/s | 765 kB     00:00    
(13/55): mariadb-server-10.3.28-1.module_el8.3.0+757+d382997d.x86_64.rpm  52 MB/s |  16 MB     00:00    
(14/55): libaio-0.3.112-1.el8.x86_64.rpm                                 358 kB/s |  33 kB     00:00    
(15/55): iproute-6.2.0-5.el8.x86_64.rpm                                  3.4 MB/s | 854 kB     00:00    
(16/55): groff-base-1.22.3-18.el8.x86_64.rpm                             3.6 MB/s | 1.0 MB     00:00    
(17/55): libmnl-1.0.4-6.el8.x86_64.rpm                                   933 kB/s |  30 kB     00:00    
(18/55): libbpf-0.5.0-1.el8.x86_64.rpm                                   1.9 MB/s | 137 kB     00:00    
(19/55): perl-Carp-1.42-396.el8.noarch.rpm                               969 kB/s |  30 kB     00:00    
(20/55): perl-Data-Dumper-2.167-399.el8.x86_64.rpm                       1.8 MB/s |  58 kB     00:00    
(21/55): perl-Errno-1.28-422.el8.x86_64.rpm                              2.4 MB/s |  76 kB     00:00    
(22/55): perl-Encode-2.97-3.el8.x86_64.rpm                                21 MB/s | 1.5 MB     00:00    
(23/55): perl-Exporter-5.72-396.el8.noarch.rpm                           895 kB/s |  34 kB     00:00    
(24/55): perl-File-Path-2.15-2.el8.noarch.rpm                            1.2 MB/s |  38 kB     00:00    
(25/55): perl-File-Temp-0.230.600-1.el8.noarch.rpm                       1.9 MB/s |  63 kB     00:00    
(26/55): perl-Getopt-Long-2.50-4.el8.noarch.rpm                          1.9 MB/s |  63 kB     00:00    
(27/55): ncurses-6.1-9.20180224.el8.x86_64.rpm                           1.9 MB/s | 387 kB     00:00    
(28/55): perl-HTTP-Tiny-0.074-1.el8.noarch.rpm                           1.2 MB/s |  58 kB     00:00    
(29/55): perl-IO-1.38-422.el8.x86_64.rpm                                 4.3 MB/s | 142 kB     00:00    
(30/55): perl-MIME-Base64-3.15-396.el8.x86_64.rpm                        998 kB/s |  31 kB     00:00    
(31/55): perl-Math-Complex-1.59-422.el8.noarch.rpm                       3.3 MB/s | 109 kB     00:00    
(32/55): perl-PathTools-3.74-1.el8.x86_64.rpm                            2.8 MB/s |  90 kB     00:00    
(33/55): perl-Math-BigInt-1.9998.11-7.el8.noarch.rpm                     3.2 MB/s | 196 kB     00:00    
(34/55): perl-Pod-Escapes-1.07-395.el8.noarch.rpm                        649 kB/s |  20 kB     00:00    
(35/55): perl-Pod-Perldoc-3.28-396.el8.noarch.rpm                        2.7 MB/s |  86 kB     00:00    
(36/55): perl-Pod-Simple-3.35-395.el8.noarch.rpm                         6.4 MB/s | 213 kB     00:00    
(37/55): perl-Pod-Usage-1.69-395.el8.noarch.rpm                          1.1 MB/s |  34 kB     00:00    
(38/55): perl-Scalar-List-Utils-1.49-2.el8.x86_64.rpm                    2.1 MB/s |  68 kB     00:00    
(39/55): perl-Socket-2.027-3.el8.x86_64.rpm                              1.7 MB/s |  59 kB     00:00    
(40/55): perl-Storable-3.11-3.el8.x86_64.rpm                             2.9 MB/s |  98 kB     00:00    
(41/55): perl-Term-ANSIColor-4.06-396.el8.noarch.rpm                     1.4 MB/s |  46 kB     00:00    
(42/55): perl-Term-Cap-1.17-395.el8.noarch.rpm                           727 kB/s |  23 kB     00:00    
(43/55): perl-Text-ParseWords-3.30-395.el8.noarch.rpm                    553 kB/s |  18 kB     00:00    
(44/55): perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch.rpm                779 kB/s |  24 kB     00:00    
(45/55): perl-Time-Local-1.280-1.el8.noarch.rpm                          1.1 MB/s |  34 kB     00:00    
(46/55): perl-Unicode-Normalize-1.25-396.el8.x86_64.rpm                  2.5 MB/s |  82 kB     00:00    
(47/55): perl-constant-1.33-396.el8.noarch.rpm                           816 kB/s |  25 kB     00:00    
(48/55): perl-macros-5.26.3-422.el8.x86_64.rpm                           2.2 MB/s |  73 kB     00:00    
(49/55): perl-libs-5.26.3-422.el8.x86_64.rpm                              32 MB/s | 1.6 MB     00:00    
(50/55): perl-parent-0.237-1.el8.noarch.rpm                              636 kB/s |  20 kB     00:00    
(51/55): perl-podlators-4.11-1.el8.noarch.rpm                            3.5 MB/s | 118 kB     00:00    
(52/55): perl-threads-2.21-2.el8.x86_64.rpm                              1.9 MB/s |  61 kB     00:00    
(53/55): perl-threads-shared-1.58-2.el8.x86_64.rpm                       1.5 MB/s |  48 kB     00:00    
(54/55): perl-interpreter-5.26.3-422.el8.x86_64.rpm                       36 MB/s | 6.3 MB     00:00    
(55/55): psmisc-23.1-5.el8.x86_64.rpm                                    2.6 MB/s | 151 kB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                     27 MB/s |  45 MB     00:01     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Installing       : mariadb-connector-c-config-3.1.11-2.el8_3.noarch                               1/55 
  Installing       : mariadb-common-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                  2/55 
  Installing       : psmisc-23.1-5.el8.x86_64                                                       3/55 
  Installing       : libaio-0.3.112-1.el8.x86_64                                                    4/55 
  Installing       : mariadb-errmsg-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                  5/55 
  Installing       : ncurses-6.1-9.20180224.el8.x86_64                                              6/55 
  Installing       : libmnl-1.0.4-6.el8.x86_64                                                      7/55 
  Running scriptlet: libmnl-1.0.4-6.el8.x86_64                                                      7/55 
  Installing       : libbpf-0.5.0-1.el8.x86_64                                                      8/55 
  Installing       : iproute-6.2.0-5.el8.x86_64                                                     9/55 
  Installing       : groff-base-1.22.3-18.el8.x86_64                                               10/55 
  Installing       : perl-Pod-Escapes-1:1.07-395.el8.noarch                                        11/55 
  Installing       : perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch                     12/55 
  Installing       : perl-Time-Local-1:1.280-1.el8.noarch                                          13/55 
  Installing       : perl-IO-Socket-IP-0.39-5.el8.noarch                                           14/55 
  Installing       : perl-Term-ANSIColor-4.06-396.el8.noarch                                       15/55 
  Installing       : perl-Term-Cap-1.17-395.el8.noarch                                             16/55 
  Installing       : perl-File-Temp-0.230.600-1.el8.noarch                                         17/55 
  Installing       : perl-Pod-Simple-1:3.35-395.el8.noarch                                         18/55 
  Installing       : perl-HTTP-Tiny-0.074-1.el8.noarch                                             19/55 
  Installing       : perl-podlators-4.11-1.el8.noarch                                              20/55 
  Installing       : perl-Pod-Perldoc-3.28-396.el8.noarch                                          21/55 
  Installing       : perl-Text-ParseWords-3.30-395.el8.noarch                                      22/55 
  Installing       : perl-Pod-Usage-4:1.69-395.el8.noarch                                          23/55 
  Installing       : perl-MIME-Base64-3.15-396.el8.x86_64                                          24/55 
  Installing       : perl-Storable-1:3.11-3.el8.x86_64                                             25/55 
  Installing       : perl-Getopt-Long-1:2.50-4.el8.noarch                                          26/55 
  Installing       : perl-Errno-1.28-422.el8.x86_64                                                27/55 
  Installing       : perl-Socket-4:2.027-3.el8.x86_64                                              28/55 
  Installing       : perl-Encode-4:2.97-3.el8.x86_64                                               29/55 
  Installing       : perl-Carp-1.42-396.el8.noarch                                                 30/55 
  Installing       : perl-Exporter-5.72-396.el8.noarch                                             31/55 
  Installing       : perl-libs-4:5.26.3-422.el8.x86_64                                             32/55 
  Installing       : perl-Scalar-List-Utils-3:1.49-2.el8.x86_64                                    33/55 
  Installing       : perl-parent-1:0.237-1.el8.noarch                                              34/55 
  Installing       : perl-macros-4:5.26.3-422.el8.x86_64                                           35/55 
  Installing       : perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch                                  36/55 
  Installing       : perl-Unicode-Normalize-1.25-396.el8.x86_64                                    37/55 
  Installing       : perl-File-Path-2.15-2.el8.noarch                                              38/55 
  Installing       : perl-IO-1.38-422.el8.x86_64                                                   39/55 
  Installing       : perl-PathTools-3.74-1.el8.x86_64                                              40/55 
  Installing       : perl-constant-1.33-396.el8.noarch                                             41/55 
  Installing       : perl-threads-1:2.21-2.el8.x86_64                                              42/55 
  Installing       : perl-threads-shared-1.58-2.el8.x86_64                                         43/55 
  Installing       : perl-interpreter-4:5.26.3-422.el8.x86_64                                      44/55 
  Installing       : perl-Data-Dumper-2.167-399.el8.x86_64                                         45/55 
  Installing       : perl-Math-Complex-1.59-422.el8.noarch                                         46/55 
  Installing       : perl-Math-BigInt-1:1.9998.11-7.el8.noarch                                     47/55 
  Installing       : perl-DBI-1.641-4.module_el8+332+132e4365.x86_64                               48/55 
  Installing       : perl-DBD-MySQL-4.046-3.module_el8+353+7103df35.x86_64                         49/55 
  Installing       : mariadb-connector-c-3.1.11-2.el8_3.x86_64                                     50/55 
  Installing       : mariadb-backup-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                 51/55 
  Installing       : mariadb-gssapi-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64          52/55 
  Installing       : mariadb-server-utils-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64           53/55 
  Running scriptlet: mariadb-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                 54/55 
  Installing       : mariadb-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                 54/55 
  Running scriptlet: mariadb-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                 54/55 
  Installing       : mariadb-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                        55/55 
  Running scriptlet: mariadb-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                        55/55 
  Verifying        : mariadb-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                         1/55 
  Verifying        : mariadb-backup-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                  2/55 
  Verifying        : mariadb-common-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                  3/55 
  Verifying        : mariadb-connector-c-3.1.11-2.el8_3.x86_64                                      4/55 
  Verifying        : mariadb-connector-c-config-3.1.11-2.el8_3.noarch                               5/55 
  Verifying        : mariadb-errmsg-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                  6/55 
  Verifying        : mariadb-gssapi-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64           7/55 
  Verifying        : mariadb-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                  8/55 
  Verifying        : mariadb-server-utils-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64            9/55 
  Verifying        : perl-DBD-MySQL-4.046-3.module_el8+353+7103df35.x86_64                         10/55 
  Verifying        : perl-DBI-1.641-4.module_el8+332+132e4365.x86_64                               11/55 
  Verifying        : perl-IO-Socket-IP-0.39-5.el8.noarch                                           12/55 
  Verifying        : perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch                     13/55 
  Verifying        : groff-base-1.22.3-18.el8.x86_64                                               14/55 
  Verifying        : iproute-6.2.0-5.el8.x86_64                                                    15/55 
  Verifying        : libaio-0.3.112-1.el8.x86_64                                                   16/55 
  Verifying        : libbpf-0.5.0-1.el8.x86_64                                                     17/55 
  Verifying        : libmnl-1.0.4-6.el8.x86_64                                                     18/55 
  Verifying        : ncurses-6.1-9.20180224.el8.x86_64                                             19/55 
  Verifying        : perl-Carp-1.42-396.el8.noarch                                                 20/55 
  Verifying        : perl-Data-Dumper-2.167-399.el8.x86_64                                         21/55 
  Verifying        : perl-Encode-4:2.97-3.el8.x86_64                                               22/55 
  Verifying        : perl-Errno-1.28-422.el8.x86_64                                                23/55 
  Verifying        : perl-Exporter-5.72-396.el8.noarch                                             24/55 
  Verifying        : perl-File-Path-2.15-2.el8.noarch                                              25/55 
  Verifying        : perl-File-Temp-0.230.600-1.el8.noarch                                         26/55 
  Verifying        : perl-Getopt-Long-1:2.50-4.el8.noarch                                          27/55 
  Verifying        : perl-HTTP-Tiny-0.074-1.el8.noarch                                             28/55 
  Verifying        : perl-IO-1.38-422.el8.x86_64                                                   29/55 
  Verifying        : perl-MIME-Base64-3.15-396.el8.x86_64                                          30/55 
  Verifying        : perl-Math-BigInt-1:1.9998.11-7.el8.noarch                                     31/55 
  Verifying        : perl-Math-Complex-1.59-422.el8.noarch                                         32/55 
  Verifying        : perl-PathTools-3.74-1.el8.x86_64                                              33/55 
  Verifying        : perl-Pod-Escapes-1:1.07-395.el8.noarch                                        34/55 
  Verifying        : perl-Pod-Perldoc-3.28-396.el8.noarch                                          35/55 
  Verifying        : perl-Pod-Simple-1:3.35-395.el8.noarch                                         36/55 
  Verifying        : perl-Pod-Usage-4:1.69-395.el8.noarch                                          37/55 
  Verifying        : perl-Scalar-List-Utils-3:1.49-2.el8.x86_64                                    38/55 
  Verifying        : perl-Socket-4:2.027-3.el8.x86_64                                              39/55 
  Verifying        : perl-Storable-1:3.11-3.el8.x86_64                                             40/55 
  Verifying        : perl-Term-ANSIColor-4.06-396.el8.noarch                                       41/55 
  Verifying        : perl-Term-Cap-1.17-395.el8.noarch                                             42/55 
  Verifying        : perl-Text-ParseWords-3.30-395.el8.noarch                                      43/55 
  Verifying        : perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch                                  44/55 
  Verifying        : perl-Time-Local-1:1.280-1.el8.noarch                                          45/55 
  Verifying        : perl-Unicode-Normalize-1.25-396.el8.x86_64                                    46/55 
  Verifying        : perl-constant-1.33-396.el8.noarch                                             47/55 
  Verifying        : perl-interpreter-4:5.26.3-422.el8.x86_64                                      48/55 
  Verifying        : perl-libs-4:5.26.3-422.el8.x86_64                                             49/55 
  Verifying        : perl-macros-4:5.26.3-422.el8.x86_64                                           50/55 
  Verifying        : perl-parent-1:0.237-1.el8.noarch                                              51/55 
  Verifying        : perl-podlators-4.11-1.el8.noarch                                              52/55 
  Verifying        : perl-threads-1:2.21-2.el8.x86_64                                              53/55 
  Verifying        : perl-threads-shared-1.58-2.el8.x86_64                                         54/55 
  Verifying        : psmisc-23.1-5.el8.x86_64                                                      55/55 
Installed products updated.

Installed:
  groff-base-1.22.3-18.el8.x86_64                                                                        
  iproute-6.2.0-5.el8.x86_64                                                                             
  libaio-0.3.112-1.el8.x86_64                                                                            
  libbpf-0.5.0-1.el8.x86_64                                                                              
  libmnl-1.0.4-6.el8.x86_64                                                                              
  mariadb-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                                 
  mariadb-backup-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                          
  mariadb-common-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                          
  mariadb-connector-c-3.1.11-2.el8_3.x86_64                                                              
  mariadb-connector-c-config-3.1.11-2.el8_3.noarch                                                       
  mariadb-errmsg-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                          
  mariadb-gssapi-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                   
  mariadb-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                          
  mariadb-server-utils-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                    
  ncurses-6.1-9.20180224.el8.x86_64                                                                      
  perl-Carp-1.42-396.el8.noarch                                                                          
  perl-DBD-MySQL-4.046-3.module_el8+353+7103df35.x86_64                                                  
  perl-DBI-1.641-4.module_el8+332+132e4365.x86_64                                                        
  perl-Data-Dumper-2.167-399.el8.x86_64                                                                  
  perl-Encode-4:2.97-3.el8.x86_64                                                                        
  perl-Errno-1.28-422.el8.x86_64                                                                         
  perl-Exporter-5.72-396.el8.noarch                                                                      
  perl-File-Path-2.15-2.el8.noarch                                                                       
  perl-File-Temp-0.230.600-1.el8.noarch                                                                  
  perl-Getopt-Long-1:2.50-4.el8.noarch                                                                   
  perl-HTTP-Tiny-0.074-1.el8.noarch                                                                      
  perl-IO-1.38-422.el8.x86_64                                                                            
  perl-IO-Socket-IP-0.39-5.el8.noarch                                                                    
  perl-MIME-Base64-3.15-396.el8.x86_64                                                                   
  perl-Math-BigInt-1:1.9998.11-7.el8.noarch                                                              
  perl-Math-Complex-1.59-422.el8.noarch                                                                  
  perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch                                              
  perl-PathTools-3.74-1.el8.x86_64                                                                       
  perl-Pod-Escapes-1:1.07-395.el8.noarch                                                                 
  perl-Pod-Perldoc-3.28-396.el8.noarch                                                                   
  perl-Pod-Simple-1:3.35-395.el8.noarch                                                                  
  perl-Pod-Usage-4:1.69-395.el8.noarch                                                                   
  perl-Scalar-List-Utils-3:1.49-2.el8.x86_64                                                             
  perl-Socket-4:2.027-3.el8.x86_64                                                                       
  perl-Storable-1:3.11-3.el8.x86_64                                                                      
  perl-Term-ANSIColor-4.06-396.el8.noarch                                                                
  perl-Term-Cap-1.17-395.el8.noarch                                                                      
  perl-Text-ParseWords-3.30-395.el8.noarch                                                               
  perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch                                                           
  perl-Time-Local-1:1.280-1.el8.noarch                                                                   
  perl-Unicode-Normalize-1.25-396.el8.x86_64                                                             
  perl-constant-1.33-396.el8.noarch                                                                      
  perl-interpreter-4:5.26.3-422.el8.x86_64                                                               
  perl-libs-4:5.26.3-422.el8.x86_64                                                                      
  perl-macros-4:5.26.3-422.el8.x86_64                                                                    
  perl-parent-1:0.237-1.el8.noarch                                                                       
  perl-podlators-4.11-1.el8.noarch                                                                       
  perl-threads-1:2.21-2.el8.x86_64                                                                       
  perl-threads-shared-1.58-2.el8.x86_64                                                                  
  psmisc-23.1-5.el8.x86_64                                                                               

Complete!
```
**5. Install `mariadb*`**
```

[root@stdb01 ~]# sudo yum install -y mariadb*
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.


Last metadata expiration check: 0:02:40 ago on Wed Nov  1 18:20:10 2023.
Package mariadb-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64 is already installed.
Package mariadb-backup-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64 is already installed.
Package mariadb-common-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64 is already installed.
Package mariadb-connector-c-3.1.11-2.el8_3.x86_64 is already installed.
Package mariadb-connector-c-config-3.1.11-2.el8_3.noarch is already installed.
Package mariadb-errmsg-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64 is already installed.
Package mariadb-gssapi-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64 is already installed.
Package mariadb-server-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64 is already installed.
Package mariadb-server-utils-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64 is already installed.
Dependencies resolved.
=========================================================================================================
 Package                      Arch   Version                                  Repository            Size
=========================================================================================================
Installing:
 mariadb-connector-c-devel    x86_64 3.1.11-2.el8_3                           appstream             68 k
 mariadb-connector-odbc       x86_64 3.1.12-1.el8                             appstream            118 k
 mariadb-devel                x86_64 3:10.3.28-1.module_el8.3.0+757+d382997d  appstream            1.1 M
 mariadb-embedded             x86_64 3:10.3.28-1.module_el8.3.0+757+d382997d  appstream            4.9 M
 mariadb-embedded-devel       x86_64 3:10.3.28-1.module_el8.3.0+757+d382997d  appstream             44 k
 mariadb-java-client          noarch 2.7.1-2.el8                              appstream            984 k
 mariadb-oqgraph-engine       x86_64 3:10.3.28-1.module_el8.3.0+757+d382997d  appstream            113 k
 mariadb-server-galera        x86_64 3:10.3.28-1.module_el8.3.0+757+d382997d  appstream             61 k
 mariadb-test                 x86_64 3:10.3.28-1.module_el8.3.0+757+d382997d  appstream             37 M
Upgrading:
 krb5-libs                    x86_64 1.18.2-25.el8_8                          ubi-8-baseos-rpms    842 k
 openssl-libs                 x86_64 1:1.1.1k-9.el8_7                         ubi-8-baseos-rpms    1.5 M
Installing dependencies:
 Judy                         x86_64 1.0.5-18.module_el8.3.0+757+d382997d     appstream            130 k
 avahi-libs                   x86_64 0.7-21.el8                               baseos                62 k
 boost-program-options        x86_64 1.66.0-13.el8                            appstream            141 k
 checkpolicy                  x86_64 2.9-1.el8                                baseos               348 k
 copy-jdk-configs             noarch 4.0-2.el8                                appstream             31 k
 cups-libs                    x86_64 1:2.2.6-54.el8                           baseos               436 k
 diffutils                    x86_64 3.6-6.el8                                baseos               358 k
 freetype                     x86_64 2.9.1-9.el8                              baseos               394 k
 galera                       x86_64 25.3.32-1.module_el8.3.0+757+d382997d    appstream            1.3 M
 java-1.8.0-openjdk-headless  x86_64 1:1.8.0.392.b08-4.el8                    ubi-8-appstream-rpms  34 M
 javapackages-filesystem      noarch 5.3.0-1.module+el8+2447+6f56d9a6         ubi-8-appstream-rpms  30 k
 jna                          x86_64 4.5.1-5.el8                              appstream            242 k
 keyutils-libs-devel          x86_64 1.5.10-9.el8                             baseos                48 k
 krb5-devel                   x86_64 1.18.2-25.el8_8                          ubi-8-baseos-rpms    562 k
 libaio-devel                 x86_64 0.3.112-1.el8                            baseos                19 k
 libcom_err-devel             x86_64 1.45.6-5.el8                             baseos                39 k
 libkadm5                     x86_64 1.18.2-25.el8_8                          ubi-8-baseos-rpms    188 k
 libpkgconf                   x86_64 1.4.2-1.el8                              baseos                35 k
 libpng                       x86_64 2:1.6.34-5.el8                           baseos               126 k
 libselinux-devel             x86_64 2.9-8.el8                                baseos               200 k
 libselinux-utils             x86_64 2.9-8.el8                                baseos               243 k
 libsepol-devel               x86_64 2.9-3.el8                                baseos                87 k
 libtool-ltdl                 x86_64 2.4.6-25.el8                             baseos                58 k
 libverto-devel               x86_64 0.3.2-2.el8                              baseos                18 k
 lksctp-tools                 x86_64 1.0.18-3.el8                             baseos               100 k
 lsof                         x86_64 4.93.2-1.el8                             baseos               253 k
 lua                          x86_64 5.3.4-12.el8                             appstream            192 k
 nspr                         x86_64 4.35.0-1.el8_8                           ubi-8-appstream-rpms 143 k
 nss                          x86_64 3.90.0-3.el8_8                           ubi-8-appstream-rpms 750 k
 nss-softokn                  x86_64 3.90.0-3.el8_8                           ubi-8-appstream-rpms 1.2 M
 nss-softokn-freebl           x86_64 3.90.0-3.el8_8                           ubi-8-appstream-rpms 389 k
 nss-sysinit                  x86_64 3.90.0-3.el8_8                           ubi-8-appstream-rpms  75 k
 nss-util                     x86_64 3.90.0-3.el8_8                           ubi-8-appstream-rpms 139 k
 openssl                      x86_64 1:1.1.1k-9.el8_7                         ubi-8-baseos-rpms    710 k
 openssl-devel                x86_64 1:1.1.1k-9.el8_7                         ubi-8-baseos-rpms    2.3 M
 pcre2-devel                  x86_64 10.32-3.el8_6                            ubi-8-baseos-rpms    605 k
 pcre2-utf16                  x86_64 10.32-3.el8_6                            ubi-8-baseos-rpms    229 k
 pcre2-utf32                  x86_64 10.32-3.el8_6                            ubi-8-baseos-rpms    220 k
 perl-Env                     noarch 1.04-395.el8                             appstream             21 k
 perl-Memoize                 noarch 1.03-422.el8                             appstream            119 k
 perl-Test-Simple             noarch 1:1.302135-1.el8                         appstream            516 k
 perl-Time-HiRes              x86_64 4:1.9758-2.el8                           appstream             61 k
 pkgconf                      x86_64 1.4.2-1.el8                              baseos                38 k
 pkgconf-m4                   noarch 1.4.2-1.el8                              baseos                17 k
 pkgconf-pkg-config           x86_64 1.4.2-1.el8                              baseos                15 k
 policycoreutils              x86_64 2.9-24.el8                               baseos               409 k
 policycoreutils-python-utils noarch 2.9-24.el8                               baseos               260 k
 python3-audit                x86_64 3.0.7-4.el8                              baseos                87 k
 python3-libselinux           x86_64 2.9-8.el8                                baseos               283 k
 python3-libsemanage          x86_64 2.9-9.el8_6                              ubi-8-baseos-rpms    128 k
 python3-policycoreutils      noarch 2.9-24.el8                               baseos               2.3 M
 python3-setools              x86_64 4.3.0-5.el8                              baseos               627 k
 rsync                        x86_64 3.1.3-19.el8_7.1                         ubi-8-baseos-rpms    410 k
 tzdata-java                  noarch 2023c-1.el8                              appstream            267 k
 unixODBC                     x86_64 2.3.7-1.el8                              appstream            458 k
 zlib-devel                   x86_64 1.2.11-21.el8_7                          ubi-8-baseos-rpms     58 k
Installing weak dependencies:
 openssl-pkcs11               x86_64 0.4.10-3.el8                             baseos                66 k
Enabling module streams:
 javapackages-runtime                201801                                                             

Transaction Summary
=========================================================================================================
Install  66 Packages
Upgrade   2 Packages

Total download size: 99 M
Downloading Packages:
(1/68): copy-jdk-configs-4.0-2.el8.noarch.rpm                            308 kB/s |  31 kB     00:00    
(2/68): Judy-1.0.5-18.module_el8.3.0+757+d382997d.x86_64.rpm             799 kB/s | 130 kB     00:00    
(3/68): boost-program-options-1.66.0-13.el8.x86_64.rpm                   845 kB/s | 141 kB     00:00    
(4/68): lua-5.3.4-12.el8.x86_64.rpm                                      2.8 MB/s | 192 kB     00:00    
(5/68): jna-4.5.1-5.el8.x86_64.rpm                                       3.1 MB/s | 242 kB     00:00    
(6/68): mariadb-connector-odbc-3.1.12-1.el8.x86_64.rpm                   2.8 MB/s | 118 kB     00:00    
(7/68): galera-25.3.32-1.module_el8.3.0+757+d382997d.x86_64.rpm          6.5 MB/s | 1.3 MB     00:00    
(8/68): mariadb-connector-c-devel-3.1.11-2.el8_3.x86_64.rpm              961 kB/s |  68 kB     00:00    
(9/68): mariadb-embedded-devel-10.3.28-1.module_el8.3.0+757+d382997d.x86 1.2 MB/s |  44 kB     00:00    
(10/68): mariadb-devel-10.3.28-1.module_el8.3.0+757+d382997d.x86_64.rpm   10 MB/s | 1.1 MB     00:00    
(11/68): mariadb-java-client-2.7.1-2.el8.noarch.rpm                       18 MB/s | 984 kB     00:00    
(12/68): mariadb-oqgraph-engine-10.3.28-1.module_el8.3.0+757+d382997d.x8 3.2 MB/s | 113 kB     00:00    
(13/68): mariadb-server-galera-10.3.28-1.module_el8.3.0+757+d382997d.x86 1.8 MB/s |  61 kB     00:00    
(14/68): perl-Env-1.04-395.el8.noarch.rpm                                581 kB/s |  21 kB     00:00    
(15/68): mariadb-embedded-10.3.28-1.module_el8.3.0+757+d382997d.x86_64.r  25 MB/s | 4.9 MB     00:00    
(16/68): perl-Memoize-1.03-422.el8.noarch.rpm                            3.2 MB/s | 119 kB     00:00    
(17/68): perl-Time-HiRes-1.9758-2.el8.x86_64.rpm                         1.6 MB/s |  61 kB     00:00    
(18/68): perl-Test-Simple-1.302135-1.el8.noarch.rpm                       11 MB/s | 516 kB     00:00    
(19/68): tzdata-java-2023c-1.el8.noarch.rpm                              6.4 MB/s | 267 kB     00:00    
(20/68): unixODBC-2.3.7-1.el8.x86_64.rpm                                  10 MB/s | 458 kB     00:00    
(21/68): avahi-libs-0.7-21.el8.x86_64.rpm                                514 kB/s |  62 kB     00:00    
(22/68): checkpolicy-2.9-1.el8.x86_64.rpm                                1.9 MB/s | 348 kB     00:00    
(23/68): cups-libs-2.2.6-54.el8.x86_64.rpm                               4.5 MB/s | 436 kB     00:00    
(24/68): diffutils-3.6-6.el8.x86_64.rpm                                   10 MB/s | 358 kB     00:00    
(25/68): freetype-2.9.1-9.el8.x86_64.rpm                                  10 MB/s | 394 kB     00:00    
(26/68): keyutils-libs-devel-1.5.10-9.el8.x86_64.rpm                     1.5 MB/s |  48 kB     00:00    
(27/68): libaio-devel-0.3.112-1.el8.x86_64.rpm                           590 kB/s |  19 kB     00:00    
(28/68): libcom_err-devel-1.45.6-5.el8.x86_64.rpm                        1.2 MB/s |  39 kB     00:00    
(29/68): libpkgconf-1.4.2-1.el8.x86_64.rpm                               1.0 MB/s |  35 kB     00:00    
(30/68): libpng-1.6.34-5.el8.x86_64.rpm                                  3.5 MB/s | 126 kB     00:00    
(31/68): mariadb-test-10.3.28-1.module_el8.3.0+757+d382997d.x86_64.rpm    56 MB/s |  37 MB     00:00    
(32/68): libselinux-devel-2.9-8.el8.x86_64.rpm                           1.1 MB/s | 200 kB     00:00    
(33/68): libselinux-utils-2.9-8.el8.x86_64.rpm                           1.3 MB/s | 243 kB     00:00    
(34/68): libtool-ltdl-2.4.6-25.el8.x86_64.rpm                            1.8 MB/s |  58 kB     00:00    
(35/68): libverto-devel-0.3.2-2.el8.x86_64.rpm                           584 kB/s |  18 kB     00:00    
(36/68): lksctp-tools-1.0.18-3.el8.x86_64.rpm                            3.0 MB/s | 100 kB     00:00    
(37/68): lsof-4.93.2-1.el8.x86_64.rpm                                    7.1 MB/s | 253 kB     00:00    
(38/68): openssl-pkcs11-0.4.10-3.el8.x86_64.rpm                          2.0 MB/s |  66 kB     00:00    
(39/68): pkgconf-1.4.2-1.el8.x86_64.rpm                                  1.2 MB/s |  38 kB     00:00    
(40/68): libsepol-devel-2.9-3.el8.x86_64.rpm                             740 kB/s |  87 kB     00:00    
(41/68): pkgconf-m4-1.4.2-1.el8.noarch.rpm                               547 kB/s |  17 kB     00:00    
(42/68): pkgconf-pkg-config-1.4.2-1.el8.x86_64.rpm                       500 kB/s |  15 kB     00:00    
(43/68): policycoreutils-python-utils-2.9-24.el8.noarch.rpm              3.6 MB/s | 260 kB     00:00    
(44/68): policycoreutils-2.9-24.el8.x86_64.rpm                           4.2 MB/s | 409 kB     00:00    
(45/68): python3-audit-3.0.7-4.el8.x86_64.rpm                            1.1 MB/s |  87 kB     00:00    
(46/68): python3-libselinux-2.9-8.el8.x86_64.rpm                         7.8 MB/s | 283 kB     00:00    
(47/68): python3-setools-4.3.0-5.el8.x86_64.rpm                           16 MB/s | 627 kB     00:00    
(48/68): python3-policycoreutils-2.9-24.el8.noarch.rpm                    24 MB/s | 2.3 MB     00:00    
(49/68): libkadm5-1.18.2-25.el8_8.x86_64.rpm                             1.1 MB/s | 188 kB     00:00    
(50/68): openssl-1.1.1k-9.el8_7.x86_64.rpm                               5.0 MB/s | 710 kB     00:00    
(51/68): krb5-devel-1.18.2-25.el8_8.x86_64.rpm                           2.6 MB/s | 562 kB     00:00    
(52/68): pcre2-devel-10.32-3.el8_6.x86_64.rpm                             20 MB/s | 605 kB     00:00    
(53/68): pcre2-utf16-10.32-3.el8_6.x86_64.rpm                            7.9 MB/s | 229 kB     00:00    
(54/68): openssl-devel-1.1.1k-9.el8_7.x86_64.rpm                          26 MB/s | 2.3 MB     00:00    
(55/68): pcre2-utf32-10.32-3.el8_6.x86_64.rpm                            5.4 MB/s | 220 kB     00:00    
(56/68): python3-libsemanage-2.9-9.el8_6.x86_64.rpm                      3.2 MB/s | 128 kB     00:00    
(57/68): rsync-3.1.3-19.el8_7.1.x86_64.rpm                                17 MB/s | 410 kB     00:00    
(58/68): zlib-devel-1.2.11-21.el8_7.x86_64.rpm                           2.9 MB/s |  58 kB     00:00    
(59/68): javapackages-filesystem-5.3.0-1.module+el8+2447+6f56d9a6.noarch 1.2 MB/s |  30 kB     00:00    
(60/68): nspr-4.35.0-1.el8_8.x86_64.rpm                                  5.4 MB/s | 143 kB     00:00    
(61/68): nss-3.90.0-3.el8_8.x86_64.rpm                                    20 MB/s | 750 kB     00:00    
(62/68): nss-softokn-3.90.0-3.el8_8.x86_64.rpm                            27 MB/s | 1.2 MB     00:00    
(63/68): nss-softokn-freebl-3.90.0-3.el8_8.x86_64.rpm                     10 MB/s | 389 kB     00:00    
(64/68): nss-sysinit-3.90.0-3.el8_8.x86_64.rpm                           4.2 MB/s |  75 kB     00:00    
(65/68): nss-util-3.90.0-3.el8_8.x86_64.rpm                              7.0 MB/s | 139 kB     00:00    
(66/68): krb5-libs-1.18.2-25.el8_8.x86_64.rpm                             22 MB/s | 842 kB     00:00    
(67/68): openssl-libs-1.1.1k-9.el8_7.x86_64.rpm                           27 MB/s | 1.5 MB     00:00    
(68/68): java-1.8.0-openjdk-headless-1.8.0.392.b08-4.el8.x86_64.rpm       69 MB/s |  34 MB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                     42 MB/s |  99 MB     00:02     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Running scriptlet: copy-jdk-configs-4.0-2.el8.noarch                                               1/1 
  Running scriptlet: java-1.8.0-openjdk-headless-1:1.8.0.392.b08-4.el8.x86_64                        1/1 
  Preparing        :                                                                                 1/1 
  Installing       : nspr-4.35.0-1.el8_8.x86_64                                                     1/70 
  Running scriptlet: nspr-4.35.0-1.el8_8.x86_64                                                     1/70 
  Installing       : nss-util-3.90.0-3.el8_8.x86_64                                                 2/70 
  Installing       : javapackages-filesystem-5.3.0-1.module+el8+2447+6f56d9a6.noarch                3/70 
  Installing       : python3-libselinux-2.9-8.el8.x86_64                                            4/70 
  Installing       : libselinux-utils-2.9-8.el8.x86_64                                              5/70 
  Installing       : python3-setools-4.3.0-5.el8.x86_64                                             6/70 
  Installing       : python3-libsemanage-2.9-9.el8_6.x86_64                                         7/70 
  Installing       : nss-softokn-freebl-3.90.0-3.el8_8.x86_64                                       8/70 
  Installing       : nss-softokn-3.90.0-3.el8_8.x86_64                                              9/70 
  Installing       : nss-3.90.0-3.el8_8.x86_64                                                     10/70 
  Installing       : nss-sysinit-3.90.0-3.el8_8.x86_64                                             11/70 
  Installing       : openssl-1:1.1.1k-9.el8_7.x86_64                                               12/70 
  Upgrading        : openssl-libs-1:1.1.1k-9.el8_7.x86_64                                          13/70 
  Running scriptlet: openssl-libs-1:1.1.1k-9.el8_7.x86_64                                          13/70 
  Installing       : openssl-pkcs11-0.4.10-3.el8.x86_64                                            14/70 
  Upgrading        : krb5-libs-1.18.2-25.el8_8.x86_64                                              15/70 
  Installing       : libkadm5-1.18.2-25.el8_8.x86_64                                               16/70 
  Installing       : mariadb-embedded-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64               17/70 
  Installing       : rsync-3.1.3-19.el8_7.1.x86_64                                                 18/70 
  Installing       : pcre2-utf32-10.32-3.el8_6.x86_64                                              19/70 
  Installing       : pcre2-utf16-10.32-3.el8_6.x86_64                                              20/70 
  Installing       : python3-audit-3.0.7-4.el8.x86_64                                              21/70 
  Installing       : pkgconf-m4-1.4.2-1.el8.noarch                                                 22/70 
  Installing       : lsof-4.93.2-1.el8.x86_64                                                      23/70 
  Installing       : lksctp-tools-1.0.18-3.el8.x86_64                                              24/70 
  Running scriptlet: lksctp-tools-1.0.18-3.el8.x86_64                                              24/70 
  Installing       : libtool-ltdl-2.4.6-25.el8.x86_64                                              25/70 
  Running scriptlet: libtool-ltdl-2.4.6-25.el8.x86_64                                              25/70 
  Installing       : unixODBC-2.3.7-1.el8.x86_64                                                   26/70 
  Running scriptlet: unixODBC-2.3.7-1.el8.x86_64                                                   26/70 
  Installing       : libpng-2:1.6.34-5.el8.x86_64                                                  27/70 
  Installing       : freetype-2.9.1-9.el8.x86_64                                                   28/70 
  Installing       : libpkgconf-1.4.2-1.el8.x86_64                                                 29/70 
  Installing       : pkgconf-1.4.2-1.el8.x86_64                                                    30/70 
  Installing       : pkgconf-pkg-config-1.4.2-1.el8.x86_64                                         31/70 
  Installing       : zlib-devel-1.2.11-21.el8_7.x86_64                                             32/70 
  Installing       : libcom_err-devel-1.45.6-5.el8.x86_64                                          33/70 
  Installing       : libsepol-devel-2.9-3.el8.x86_64                                               34/70 
  Installing       : libverto-devel-0.3.2-2.el8.x86_64                                             35/70 
  Installing       : pcre2-devel-10.32-3.el8_6.x86_64                                              36/70 
  Installing       : libselinux-devel-2.9-8.el8.x86_64                                             37/70 
  Installing       : libaio-devel-0.3.112-1.el8.x86_64                                             38/70 
  Installing       : keyutils-libs-devel-1.5.10-9.el8.x86_64                                       39/70 
  Installing       : krb5-devel-1.18.2-25.el8_8.x86_64                                             40/70 
  Installing       : openssl-devel-1:1.1.1k-9.el8_7.x86_64                                         41/70 
  Installing       : mariadb-devel-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                  42/70 
  Installing       : mariadb-connector-c-devel-3.1.11-2.el8_3.x86_64                               43/70 
  Installing       : diffutils-3.6-6.el8.x86_64                                                    44/70 
  Running scriptlet: diffutils-3.6-6.el8.x86_64                                                    44/70 
  Installing       : policycoreutils-2.9-24.el8.x86_64                                             45/70 
  Running scriptlet: policycoreutils-2.9-24.el8.x86_64                                             45/70 
  Installing       : checkpolicy-2.9-1.el8.x86_64                                                  46/70 
  Installing       : python3-policycoreutils-2.9-24.el8.noarch                                     47/70 
  Installing       : policycoreutils-python-utils-2.9-24.el8.noarch                                48/70 
  Installing       : avahi-libs-0.7-21.el8.x86_64                                                  49/70 
  Installing       : cups-libs-1:2.2.6-54.el8.x86_64                                               50/70 
  Installing       : tzdata-java-2023c-1.el8.noarch                                                51/70 
  Installing       : perl-Time-HiRes-4:1.9758-2.el8.x86_64                                         52/70 
  Installing       : perl-Test-Simple-1:1.302135-1.el8.noarch                                      53/70 
  Installing       : perl-Memoize-1.03-422.el8.noarch                                              54/70 
  Installing       : perl-Env-1.04-395.el8.noarch                                                  55/70 
  Installing       : lua-5.3.4-12.el8.x86_64                                                       56/70 
  Installing       : copy-jdk-configs-4.0-2.el8.noarch                                             57/70 
  Installing       : java-1.8.0-openjdk-headless-1:1.8.0.392.b08-4.el8.x86_64                      58/70 
  Running scriptlet: java-1.8.0-openjdk-headless-1:1.8.0.392.b08-4.el8.x86_64                      58/70 
  Installing       : jna-4.5.1-5.el8.x86_64                                                        59/70 
  Installing       : boost-program-options-1.66.0-13.el8.x86_64                                    60/70 
  Running scriptlet: boost-program-options-1.66.0-13.el8.x86_64                                    60/70 
  Installing       : galera-25.3.32-1.module_el8.3.0+757+d382997d.x86_64                           61/70 
  Running scriptlet: galera-25.3.32-1.module_el8.3.0+757+d382997d.x86_64                           61/70 
  Installing       : Judy-1.0.5-18.module_el8.3.0+757+d382997d.x86_64                              62/70 
  Installing       : mariadb-oqgraph-engine-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64         63/70 
  Installing       : mariadb-server-galera-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64          64/70 
  Running scriptlet: mariadb-server-galera-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64          64/70 
  Installing       : mariadb-java-client-2.7.1-2.el8.noarch                                        65/70 
  Installing       : mariadb-test-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                   66/70 
  Installing       : mariadb-embedded-devel-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64         67/70 
  Installing       : mariadb-connector-odbc-3.1.12-1.el8.x86_64                                    68/70 
  Cleanup          : krb5-libs-1.18.2-22.el8_7.x86_64                                              69/70 
  Cleanup          : openssl-libs-1:1.1.1k-7.el8_6.x86_64                                          70/70 
  Running scriptlet: openssl-libs-1:1.1.1k-7.el8_6.x86_64                                          70/70 
  Running scriptlet: nss-3.90.0-3.el8_8.x86_64                                                     70/70 
  Running scriptlet: copy-jdk-configs-4.0-2.el8.noarch                                             70/70 
  Running scriptlet: java-1.8.0-openjdk-headless-1:1.8.0.392.b08-4.el8.x86_64                      70/70 
  Running scriptlet: openssl-libs-1:1.1.1k-7.el8_6.x86_64                                          70/70 
  Verifying        : Judy-1.0.5-18.module_el8.3.0+757+d382997d.x86_64                               1/70 
  Verifying        : boost-program-options-1.66.0-13.el8.x86_64                                     2/70 
  Verifying        : copy-jdk-configs-4.0-2.el8.noarch                                              3/70 
  Verifying        : galera-25.3.32-1.module_el8.3.0+757+d382997d.x86_64                            4/70 
  Verifying        : jna-4.5.1-5.el8.x86_64                                                         5/70 
  Verifying        : lua-5.3.4-12.el8.x86_64                                                        6/70 
  Verifying        : mariadb-connector-c-devel-3.1.11-2.el8_3.x86_64                                7/70 
  Verifying        : mariadb-connector-odbc-3.1.12-1.el8.x86_64                                     8/70 
  Verifying        : mariadb-devel-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                   9/70 
  Verifying        : mariadb-embedded-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64               10/70 
  Verifying        : mariadb-embedded-devel-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64         11/70 
  Verifying        : mariadb-java-client-2.7.1-2.el8.noarch                                        12/70 
  Verifying        : mariadb-oqgraph-engine-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64         13/70 
  Verifying        : mariadb-server-galera-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64          14/70 
  Verifying        : mariadb-test-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                   15/70 
  Verifying        : perl-Env-1.04-395.el8.noarch                                                  16/70 
  Verifying        : perl-Memoize-1.03-422.el8.noarch                                              17/70 
  Verifying        : perl-Test-Simple-1:1.302135-1.el8.noarch                                      18/70 
  Verifying        : perl-Time-HiRes-4:1.9758-2.el8.x86_64                                         19/70 
  Verifying        : tzdata-java-2023c-1.el8.noarch                                                20/70 
  Verifying        : unixODBC-2.3.7-1.el8.x86_64                                                   21/70 
  Verifying        : avahi-libs-0.7-21.el8.x86_64                                                  22/70 
  Verifying        : checkpolicy-2.9-1.el8.x86_64                                                  23/70 
  Verifying        : cups-libs-1:2.2.6-54.el8.x86_64                                               24/70 
  Verifying        : diffutils-3.6-6.el8.x86_64                                                    25/70 
  Verifying        : freetype-2.9.1-9.el8.x86_64                                                   26/70 
  Verifying        : keyutils-libs-devel-1.5.10-9.el8.x86_64                                       27/70 
  Verifying        : libaio-devel-0.3.112-1.el8.x86_64                                             28/70 
  Verifying        : libcom_err-devel-1.45.6-5.el8.x86_64                                          29/70 
  Verifying        : libpkgconf-1.4.2-1.el8.x86_64                                                 30/70 
  Verifying        : libpng-2:1.6.34-5.el8.x86_64                                                  31/70 
  Verifying        : libselinux-devel-2.9-8.el8.x86_64                                             32/70 
  Verifying        : libselinux-utils-2.9-8.el8.x86_64                                             33/70 
  Verifying        : libsepol-devel-2.9-3.el8.x86_64                                               34/70 
  Verifying        : libtool-ltdl-2.4.6-25.el8.x86_64                                              35/70 
  Verifying        : libverto-devel-0.3.2-2.el8.x86_64                                             36/70 
  Verifying        : lksctp-tools-1.0.18-3.el8.x86_64                                              37/70 
  Verifying        : lsof-4.93.2-1.el8.x86_64                                                      38/70 
  Verifying        : openssl-pkcs11-0.4.10-3.el8.x86_64                                            39/70 
  Verifying        : pkgconf-1.4.2-1.el8.x86_64                                                    40/70 
  Verifying        : pkgconf-m4-1.4.2-1.el8.noarch                                                 41/70 
  Verifying        : pkgconf-pkg-config-1.4.2-1.el8.x86_64                                         42/70 
  Verifying        : policycoreutils-2.9-24.el8.x86_64                                             43/70 
  Verifying        : policycoreutils-python-utils-2.9-24.el8.noarch                                44/70 
  Verifying        : python3-audit-3.0.7-4.el8.x86_64                                              45/70 
  Verifying        : python3-libselinux-2.9-8.el8.x86_64                                           46/70 
  Verifying        : python3-policycoreutils-2.9-24.el8.noarch                                     47/70 
  Verifying        : python3-setools-4.3.0-5.el8.x86_64                                            48/70 
  Verifying        : krb5-devel-1.18.2-25.el8_8.x86_64                                             49/70 
  Verifying        : libkadm5-1.18.2-25.el8_8.x86_64                                               50/70 
  Verifying        : openssl-1:1.1.1k-9.el8_7.x86_64                                               51/70 
  Verifying        : openssl-devel-1:1.1.1k-9.el8_7.x86_64                                         52/70 
  Verifying        : pcre2-devel-10.32-3.el8_6.x86_64                                              53/70 
  Verifying        : pcre2-utf16-10.32-3.el8_6.x86_64                                              54/70 
  Verifying        : pcre2-utf32-10.32-3.el8_6.x86_64                                              55/70 
  Verifying        : python3-libsemanage-2.9-9.el8_6.x86_64                                        56/70 
  Verifying        : rsync-3.1.3-19.el8_7.1.x86_64                                                 57/70 
  Verifying        : zlib-devel-1.2.11-21.el8_7.x86_64                                             58/70 
  Verifying        : javapackages-filesystem-5.3.0-1.module+el8+2447+6f56d9a6.noarch               59/70 
  Verifying        : java-1.8.0-openjdk-headless-1:1.8.0.392.b08-4.el8.x86_64                      60/70 
  Verifying        : nspr-4.35.0-1.el8_8.x86_64                                                    61/70 
  Verifying        : nss-3.90.0-3.el8_8.x86_64                                                     62/70 
  Verifying        : nss-softokn-3.90.0-3.el8_8.x86_64                                             63/70 
  Verifying        : nss-softokn-freebl-3.90.0-3.el8_8.x86_64                                      64/70 
  Verifying        : nss-sysinit-3.90.0-3.el8_8.x86_64                                             65/70 
  Verifying        : nss-util-3.90.0-3.el8_8.x86_64                                                66/70 
  Verifying        : krb5-libs-1.18.2-25.el8_8.x86_64                                              67/70 
  Verifying        : krb5-libs-1.18.2-22.el8_7.x86_64                                              68/70 
  Verifying        : openssl-libs-1:1.1.1k-9.el8_7.x86_64                                          69/70 
  Verifying        : openssl-libs-1:1.1.1k-7.el8_6.x86_64                                          70/70 
Installed products updated.

Upgraded:
  krb5-libs-1.18.2-25.el8_8.x86_64                  openssl-libs-1:1.1.1k-9.el8_7.x86_64                 
Installed:
  Judy-1.0.5-18.module_el8.3.0+757+d382997d.x86_64                                                       
  avahi-libs-0.7-21.el8.x86_64                                                                           
  boost-program-options-1.66.0-13.el8.x86_64                                                             
  checkpolicy-2.9-1.el8.x86_64                                                                           
  copy-jdk-configs-4.0-2.el8.noarch                                                                      
  cups-libs-1:2.2.6-54.el8.x86_64                                                                        
  diffutils-3.6-6.el8.x86_64                                                                             
  freetype-2.9.1-9.el8.x86_64                                                                            
  galera-25.3.32-1.module_el8.3.0+757+d382997d.x86_64                                                    
  java-1.8.0-openjdk-headless-1:1.8.0.392.b08-4.el8.x86_64                                               
  javapackages-filesystem-5.3.0-1.module+el8+2447+6f56d9a6.noarch                                        
  jna-4.5.1-5.el8.x86_64                                                                                 
  keyutils-libs-devel-1.5.10-9.el8.x86_64                                                                
  krb5-devel-1.18.2-25.el8_8.x86_64                                                                      
  libaio-devel-0.3.112-1.el8.x86_64                                                                      
  libcom_err-devel-1.45.6-5.el8.x86_64                                                                   
  libkadm5-1.18.2-25.el8_8.x86_64                                                                        
  libpkgconf-1.4.2-1.el8.x86_64                                                                          
  libpng-2:1.6.34-5.el8.x86_64                                                                           
  libselinux-devel-2.9-8.el8.x86_64                                                                      
  libselinux-utils-2.9-8.el8.x86_64                                                                      
  libsepol-devel-2.9-3.el8.x86_64                                                                        
  libtool-ltdl-2.4.6-25.el8.x86_64                                                                       
  libverto-devel-0.3.2-2.el8.x86_64                                                                      
  lksctp-tools-1.0.18-3.el8.x86_64                                                                       
  lsof-4.93.2-1.el8.x86_64                                                                               
  lua-5.3.4-12.el8.x86_64                                                                                
  mariadb-connector-c-devel-3.1.11-2.el8_3.x86_64                                                        
  mariadb-connector-odbc-3.1.12-1.el8.x86_64                                                             
  mariadb-devel-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                           
  mariadb-embedded-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                        
  mariadb-embedded-devel-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                  
  mariadb-java-client-2.7.1-2.el8.noarch                                                                 
  mariadb-oqgraph-engine-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                  
  mariadb-server-galera-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                   
  mariadb-test-3:10.3.28-1.module_el8.3.0+757+d382997d.x86_64                                            
  nspr-4.35.0-1.el8_8.x86_64                                                                             
  nss-3.90.0-3.el8_8.x86_64                                                                              
  nss-softokn-3.90.0-3.el8_8.x86_64                                                                      
  nss-softokn-freebl-3.90.0-3.el8_8.x86_64                                                               
  nss-sysinit-3.90.0-3.el8_8.x86_64                                                                      
  nss-util-3.90.0-3.el8_8.x86_64                                                                         
  openssl-1:1.1.1k-9.el8_7.x86_64                                                                        
  openssl-devel-1:1.1.1k-9.el8_7.x86_64                                                                  
  openssl-pkcs11-0.4.10-3.el8.x86_64                                                                     
  pcre2-devel-10.32-3.el8_6.x86_64                                                                       
  pcre2-utf16-10.32-3.el8_6.x86_64                                                                       
  pcre2-utf32-10.32-3.el8_6.x86_64                                                                       
  perl-Env-1.04-395.el8.noarch                                                                           
  perl-Memoize-1.03-422.el8.noarch                                                                       
  perl-Test-Simple-1:1.302135-1.el8.noarch                                                               
  perl-Time-HiRes-4:1.9758-2.el8.x86_64                                                                  
  pkgconf-1.4.2-1.el8.x86_64                                                                             
  pkgconf-m4-1.4.2-1.el8.noarch                                                                          
  pkgconf-pkg-config-1.4.2-1.el8.x86_64                                                                  
  policycoreutils-2.9-24.el8.x86_64                                                                      
  policycoreutils-python-utils-2.9-24.el8.noarch                                                         
  python3-audit-3.0.7-4.el8.x86_64                                                                       
  python3-libselinux-2.9-8.el8.x86_64                                                                    
  python3-libsemanage-2.9-9.el8_6.x86_64                                                                 
  python3-policycoreutils-2.9-24.el8.noarch                                                              
  python3-setools-4.3.0-5.el8.x86_64                                                                     
  rsync-3.1.3-19.el8_7.1.x86_64                                                                          
  tzdata-java-2023c-1.el8.noarch                                                                         
  unixODBC-2.3.7-1.el8.x86_64                                                                            
  zlib-devel-1.2.11-21.el8_7.x86_64                                                                      

Complete!
```
**6. Restart , enable, start and status of mariadb service**
```

[root@stdb01 ~]# systemctl restart mariadb
[root@stdb01 ~]# sudo systemctl enable mariadb && systemctl start mariadb && systemctl status mariadb
Created symlink /etc/systemd/system/mysql.service → /usr/lib/systemd/system/mariadb.service.
Created symlink /etc/systemd/system/mysqld.service → /usr/lib/systemd/system/mariadb.service.
Created symlink /etc/systemd/system/multi-user.target.wants/mariadb.service → /usr/lib/systemd/system/mariadb.service.
● mariadb.service - MariaDB 10.3 database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2023-11-01 18:23:36 UTC; 6s ago
     Docs: man:mysqld(8)
           https://mariadb.com/kb/en/library/systemd/
 Main PID: 2564 (mysqld)
   Status: "Taking your SQL requests now..."
    Tasks: 30 (limit: 1340692)
   Memory: 102.5M
   CGroup: /docker/c7dd3650fd6d3b80e7b7709e45424e6bdcc440314fe3a6958128c37f75627094/system.slice/mariadb.
service
           └─2564 /usr/libexec/mysqld --basedir=/usr

Nov 01 18:23:36 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Job m
ariadb.service/start finished, result=done
Nov 01 18:23:36 stdb01.stratos.xfusioncorp.com systemd[1]: Started MariaDB 10.3 database server.
Nov 01 18:23:36 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Faile
d to send unit change signal for mariadb.service: Connection reset by peer
Nov 01 18:23:42 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Unkno
wn serialization key: ref-gid
Nov 01 18:23:42 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Chang
ed dead -> running
Nov 01 18:23:42 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Faile
d to set invocation ID on control group /docker/c7dd3650fd6d3b80e7b7709e45424e6bdcc440314fe3a6958128c37f7
5627094/system.slice/mariadb.service, ignoring: Operation not permitted
Nov 01 18:23:42 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Tryin
g to enqueue job mariadb.service/start/replace
Nov 01 18:23:42 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Insta
lled new job mariadb.service/start as 123
Nov 01 18:23:42 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Enque
ued job mariadb.service/start as 123
Nov 01 18:23:42 stdb01.stratos.xfusioncorp.com systemd[1]: mariadb.service: Job m
ariadb.service/start finished, result=done
```
**7. Secure MariaDB in CentOS 7**
```

[root@stdb01 ~]# sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y
New password: 123
Re-enter new password: 123
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```
**8. Login to the MariaDB command**
```

[root@stdb01 ~]# mysql -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 16
Server version: 10.3.28-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

```
 - show database
```
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.000 sec)
```
- create a database, named, kodekloud_db3
```
MariaDB [(none)]> CREATE DATABASE kodekloud_db3;
Query OK, 1 row affected (0.000 sec)

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| kodekloud_db3      |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.000 sec)
```
- Create a user kodekloud_rin and set any password you like and grant full permissions to user kodekloud_rin on database kodekloud_db3.
```
MariaDB [(none)]> CREATE USER 'kodekloud_rin'@localhost IDENTIFIED BY 'LQfKeWWxWD';
Query OK, 0 rows affected (0.000 sec)

MariaDB [(none)]> GRANT ALL PRIVILEGES ON kodekloud_db3.* TO 'kodekloud_rin'@'localhost';
Query OK, 0 rows affected (0.000 sec)
```
- Verify that user got the necessary permissions,
```
MariaDB [(none)]> SHOW GRANTS for 'kodekloud_rin'@'localhost';
+----------------------------------------------------------------------------------------------------------------------+
| Grants for kodekloud_rin@localhost                                                                                   |
+----------------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `kodekloud_rin`@`localhost` IDENTIFIED BY PASSWORD '*F1CAF4084DFE2FC0ECBD3F62509FEF5D2F127F14' |
| GRANT ALL PRIVILEGES ON `kodekloud_db3`.* TO `kodekloud_rin`@`localhost`                                             |
+----------------------------------------------------------------------------------------------------------------------+
2 rows in set (0.000 sec)
```
- If you run any command on the MySQL/MariaDB server you must run the below command each time. All changes will take effect once you run the command below.
```
MariaDB [(none)]> FLUSH PRIVILEGES;
```
![1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/9325b28f-c1f1-4692-b095-d19f028bf8ea)

**9. Click on `Finish` & `Confirm` to complete the task successful**

![2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/792401aa-eaa4-48f7-8a58-bb3b94f1d67c)