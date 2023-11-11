
## Question

The  `Nautilus`  application development team is planning to launch a new PHP-based application, which they want to deploy on  `Nautilus`  infra in  `Stratos DC`. The development team had a meeting with the production support team and they have shared some requirements regarding the infrastructure. Below are the requirements they shared:  
  

  

a. Install  `nginx`  on  `app server 1`  , configure it to use port  `8096`  and its document root should be  `/var/www/html`.  
  

b. Install  `php-fpm`  version  `7.2`  on  `app server 1`, it should listen on port  `9000`.  
  

c. Configure php-fpm and nginx to work together.  
  

d. Once configured correctly, you can test the website using  `curl http://stapp01:8096/index.php`  command from jump host.


## Solution:

**1. Login on App server as per the task & switch to root user**

```

thor@jump_host ~$ ssh tony@stapp01

tony@stapp01's password: Ir0nM@n
[tony@stapp01 ~]$ sudo su -

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.


[sudo] password for tony: Ir0nM@n
[root@stapp01 ~]#

```

**2. To install Nginx we need to install epel repo first**

```
[root@stapp01 ~]# yum install epel-release
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                               14 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                               25 MB/s |  34 MB     00:01    
CentOS Stream 8 - BaseOS                                                  36 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                  51 MB/s |  53 MB     00:01    
CentOS Stream 8 - Extras                                                  14 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                  22 kB/s | 3.0 kB     00:00    
CentOS Stream 8 - Extras common packages                                  43 kB/s | 6.9 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                           2.7 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                         15 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                613 kB/s |  99 kB     00:00    
Dependencies resolved.
=========================================================================================================
 Package                     Architecture          Version                   Repository             Size
=========================================================================================================
Installing:
 epel-release                noarch                8-11.el8                  extras                 24 k

Transaction Summary
=========================================================================================================
Install  1 Package

Total download size: 24 k
Installed size: 35 k
Is this ok [y/N]: y
Downloading Packages:
epel-release-8-11.el8.noarch.rpm                                         161 kB/s |  24 kB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                     97 kB/s |  24 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Installing       : epel-release-8-11.el8.noarch                                                    1/1 
  Running scriptlet: epel-release-8-11.el8.noarch                                                    1/1 
  Verifying        : epel-release-8-11.el8.noarch                                                    1/1 
Installed products updated.

Installed:
  epel-release-8-11.el8.noarch                                                                           

Complete!
[root@stapp03 ~]# 
```

**3. Now Install Nginx**

```
[root@stapp01 ~]# yum install nginx
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Extra Packages for Enterprise Linux 8 - x86_64                            17 MB/s |  16 MB     00:00    
Extra Packages for Enterprise Linux Modular 8 - x86_64                   1.1 MB/s | 733 kB     00:00    
Dependencies resolved.
=========================================================================================================
 Package                     Arch   Version                                   Repository            Size
=========================================================================================================
Installing:
 nginx                       x86_64 1:1.14.1-9.module+el8.0.0+4108+af250afe   ubi-8-appstream-rpms 570 k
Upgrading:
 openssl-libs                x86_64 1:1.1.1k-9.el8_7                          ubi-8-baseos-rpms    1.5 M
Installing dependencies:
 dejavu-fonts-common         noarch 2.35-7.el8                                baseos                74 k
 dejavu-sans-fonts           noarch 2.35-7.el8                                baseos               1.6 M
 fontconfig                  x86_64 2.13.1-4.el8                              baseos               274 k
 fontpackages-filesystem     noarch 1.44-22.el8                               baseos                16 k
 freetype                    x86_64 2.9.1-9.el8                               baseos               394 k
 gd                          x86_64 2.2.5-7.el8                               appstream            144 k
 groff-base                  x86_64 1.22.3-18.el8                             baseos               1.0 M
 jbigkit-libs                x86_64 2.1-14.el8                                appstream             55 k
 libX11                      x86_64 1.6.8-7.el8                               appstream            612 k
 libX11-common               noarch 1.6.8-7.el8                               appstream            211 k
 libXau                      x86_64 1.0.9-3.el8                               appstream             37 k
 libXpm                      x86_64 3.5.12-11.el8                             appstream             59 k
 libjpeg-turbo               x86_64 1.5.3-12.el8                              appstream            157 k
 libpng                      x86_64 2:1.6.34-5.el8                            baseos               126 k
 libtiff                     x86_64 4.0.9-29.el8_8                            ubi-8-appstream-rpms 189 k
 libwebp                     x86_64 1.0.0-9.el8                               appstream            274 k
 libxcb                      x86_64 1.13.1-1.el8                              appstream            229 k
 libxslt                     x86_64 1.1.32-6.el8                              baseos               250 k
 ncurses                     x86_64 6.1-9.20180224.el8                        baseos               387 k
 nginx-all-modules           noarch 1:1.14.1-9.module+el8.0.0+4108+af250afe   ubi-8-appstream-rpms  24 k
 nginx-filesystem            noarch 1:1.14.1-9.module+el8.0.0+4108+af250afe   ubi-8-appstream-rpms  24 k
 nginx-mod-http-image-filter x86_64 1:1.14.1-9.module+el8.0.0+4108+af250afe   ubi-8-appstream-rpms  35 k
 nginx-mod-http-perl         x86_64 1:1.14.1-9.module+el8.0.0+4108+af250afe   ubi-8-appstream-rpms  46 k
 nginx-mod-http-xslt-filter  x86_64 1:1.14.1-9.module+el8.0.0+4108+af250afe   ubi-8-appstream-rpms  34 k
 nginx-mod-mail              x86_64 1:1.14.1-9.module+el8.0.0+4108+af250afe   ubi-8-appstream-rpms  64 k
 nginx-mod-stream            x86_64 1:1.14.1-9.module+el8.0.0+4108+af250afe   ubi-8-appstream-rpms  85 k
 openssl                     x86_64 1:1.1.1k-9.el8_7                          ubi-8-baseos-rpms    710 k
 perl-Carp                   noarch 1.42-396.el8                              baseos                30 k
 perl-Data-Dumper            x86_64 2.167-399.el8                             baseos                58 k
 perl-Digest                 noarch 1.17-395.el8                              appstream             27 k
 perl-Digest-MD5             x86_64 2.55-396.el8                              appstream             37 k
 perl-Encode                 x86_64 4:2.97-3.el8                              baseos               1.5 M
 perl-Errno                  x86_64 1.28-422.el8                              baseos                76 k
 perl-Exporter               noarch 5.72-396.el8                              baseos                34 k
 perl-File-Path              noarch 2.15-2.el8                                baseos                38 k
 perl-File-Temp              noarch 0.230.600-1.el8                           baseos                63 k
 perl-Getopt-Long            noarch 1:2.50-4.el8                              baseos                63 k
 perl-HTTP-Tiny              noarch 0.074-2.el8                               baseos                58 k
 perl-IO                     x86_64 1.38-422.el8                              baseos               142 k
 perl-IO-Socket-IP           noarch 0.39-5.el8                                appstream             47 k
 perl-IO-Socket-SSL          noarch 2.066-4.module_el8+339+1ec643e0           appstream            304 k
 perl-MIME-Base64            x86_64 3.15-396.el8                              baseos                31 k
 perl-Mozilla-CA             noarch 20160104-7.module_el8+645+9d809f8c        appstream             15 k
 perl-Net-SSLeay             x86_64 1.88-2.module_el8+339+1ec643e0            appstream            402 k
 perl-PathTools              x86_64 3.74-1.el8                                baseos                90 k
 perl-Pod-Escapes            noarch 1:1.07-395.el8                            baseos                20 k
 perl-Pod-Perldoc            noarch 3.28-396.el8                              baseos                86 k
 perl-Pod-Simple             noarch 1:3.35-395.el8                            baseos               213 k
 perl-Pod-Usage              noarch 4:1.69-395.el8                            baseos                34 k
 perl-Scalar-List-Utils      x86_64 3:1.49-2.el8                              baseos                68 k
 perl-Socket                 x86_64 4:2.027-3.el8                             baseos                59 k
 perl-Storable               x86_64 1:3.11-3.el8                              baseos                98 k
 perl-Term-ANSIColor         noarch 4.06-396.el8                              baseos                46 k
 perl-Term-Cap               noarch 1.17-395.el8                              baseos                23 k
 perl-Text-ParseWords        noarch 3.30-395.el8                              baseos                18 k
 perl-Text-Tabs+Wrap         noarch 2013.0523-395.el8                         baseos                24 k
 perl-Time-Local             noarch 1:1.280-1.el8                             baseos                34 k
 perl-URI                    noarch 1.73-3.el8                                appstream            116 k
 perl-Unicode-Normalize      x86_64 1.25-396.el8                              baseos                82 k
 perl-constant               noarch 1.33-396.el8                              baseos                25 k
 perl-interpreter            x86_64 4:5.26.3-422.el8                          baseos               6.3 M
 perl-libnet                 noarch 3.11-3.el8                                appstream            121 k
 perl-libs                   x86_64 4:5.26.3-422.el8                          baseos               1.6 M
 perl-macros                 x86_64 4:5.26.3-422.el8                          baseos                73 k
 perl-parent                 noarch 1:0.237-1.el8                             baseos                20 k
 perl-podlators              noarch 4.11-1.el8                                baseos               118 k
 perl-threads                x86_64 1:2.21-2.el8                              baseos                61 k
 perl-threads-shared         x86_64 1.58-2.el8                                baseos                48 k
Installing weak dependencies:
 openssl-pkcs11              x86_64 0.4.10-3.el8                              baseos                66 k
Enabling module streams:
 nginx                              1.14                                                                
 perl                               5.26                                                                
 perl-IO-Socket-SSL                 2.066                                                               
 perl-libwww-perl                   6.34                                                                

Transaction Summary
=========================================================================================================
Install  70 Packages
Upgrade   1 Package

Total download size: 21 M
Is this ok [y/N]: y
Downloading Packages:
(1/71): jbigkit-libs-2.1-14.el8.x86_64.rpm                               247 kB/s |  55 kB     00:00    
(2/71): gd-2.2.5-7.el8.x86_64.rpm                                        523 kB/s | 144 kB     00:00    
(3/71): libXau-1.0.9-3.el8.x86_64.rpm                                    666 kB/s |  37 kB     00:00    
(4/71): libX11-1.6.8-7.el8.x86_64.rpm                                    1.5 MB/s | 612 kB     00:00    
(5/71): libX11-common-1.6.8-7.el8.noarch.rpm                             1.2 MB/s | 211 kB     00:00    
(6/71): libXpm-3.5.12-11.el8.x86_64.rpm                                  944 kB/s |  59 kB     00:00    
(7/71): libjpeg-turbo-1.5.3-12.el8.x86_64.rpm                            2.7 MB/s | 157 kB     00:00    
(8/71): libwebp-1.0.0-9.el8.x86_64.rpm                                   4.5 MB/s | 274 kB     00:00    
(9/71): perl-Digest-1.17-395.el8.noarch.rpm                              477 kB/s |  27 kB     00:00    
(10/71): libxcb-1.13.1-1.el8.x86_64.rpm                                  2.0 MB/s | 229 kB     00:00    
(11/71): perl-Digest-MD5-2.55-396.el8.x86_64.rpm                         631 kB/s |  37 kB     00:00    
(12/71): perl-IO-Socket-IP-0.39-5.el8.noarch.rpm                         828 kB/s |  47 kB     00:00    
(13/71): perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch.rpm   273 kB/s |  15 kB     00:00    
(14/71): perl-IO-Socket-SSL-2.066-4.module_el8+339+1ec643e0.noarch.rpm   4.8 MB/s | 304 kB     00:00    
(15/71): perl-URI-1.73-3.el8.noarch.rpm                                  2.0 MB/s | 116 kB     00:00    
(16/71): perl-libnet-3.11-3.el8.noarch.rpm                               2.0 MB/s | 121 kB     00:00    
(17/71): perl-Net-SSLeay-1.88-2.module_el8+339+1ec643e0.x86_64.rpm       3.5 MB/s | 402 kB     00:00    
(18/71): dejavu-fonts-common-2.35-7.el8.noarch.rpm                       859 kB/s |  74 kB     00:00    
(19/71): fontpackages-filesystem-1.44-22.el8.noarch.rpm                  838 kB/s |  16 kB     00:00    
(20/71): fontconfig-2.13.1-4.el8.x86_64.rpm                              2.5 MB/s | 274 kB     00:00    
(21/71): freetype-2.9.1-9.el8.x86_64.rpm                                 6.8 MB/s | 394 kB     00:00    
(22/71): dejavu-sans-fonts-2.35-7.el8.noarch.rpm                         9.2 MB/s | 1.6 MB     00:00    
(23/71): libpng-1.6.34-5.el8.x86_64.rpm                                  6.0 MB/s | 126 kB     00:00    
(24/71): libxslt-1.1.32-6.el8.x86_64.rpm                                  10 MB/s | 250 kB     00:00    
(25/71): groff-base-1.22.3-18.el8.x86_64.rpm                              17 MB/s | 1.0 MB     00:00    
(26/71): openssl-pkcs11-0.4.10-3.el8.x86_64.rpm                          3.4 MB/s |  66 kB     00:00    
(27/71): ncurses-6.1-9.20180224.el8.x86_64.rpm                           7.6 MB/s | 387 kB     00:00    
(28/71): perl-Carp-1.42-396.el8.noarch.rpm                               1.5 MB/s |  30 kB     00:00    
(29/71): perl-Data-Dumper-2.167-399.el8.x86_64.rpm                       3.0 MB/s |  58 kB     00:00    
(30/71): perl-Errno-1.28-422.el8.x86_64.rpm                              3.6 MB/s |  76 kB     00:00    
(31/71): perl-Exporter-5.72-396.el8.noarch.rpm                           1.6 MB/s |  34 kB     00:00    
(32/71): perl-Encode-2.97-3.el8.x86_64.rpm                                28 MB/s | 1.5 MB     00:00    
(33/71): perl-File-Path-2.15-2.el8.noarch.rpm                            1.2 MB/s |  38 kB     00:00    
(34/71): perl-File-Temp-0.230.600-1.el8.noarch.rpm                       2.0 MB/s |  63 kB     00:00    
(35/71): perl-Getopt-Long-2.50-4.el8.noarch.rpm                          3.1 MB/s |  63 kB     00:00    
(36/71): perl-HTTP-Tiny-0.074-2.el8.noarch.rpm                           2.9 MB/s |  58 kB     00:00    
(37/71): perl-IO-1.38-422.el8.x86_64.rpm                                 6.5 MB/s | 142 kB     00:00    
(38/71): perl-MIME-Base64-3.15-396.el8.x86_64.rpm                        1.6 MB/s |  31 kB     00:00    
(39/71): perl-PathTools-3.74-1.el8.x86_64.rpm                            4.4 MB/s |  90 kB     00:00    
(40/71): perl-Pod-Escapes-1.07-395.el8.noarch.rpm                        1.1 MB/s |  20 kB     00:00    
(41/71): perl-Pod-Perldoc-3.28-396.el8.noarch.rpm                        4.3 MB/s |  86 kB     00:00    
(42/71): perl-Pod-Simple-3.35-395.el8.noarch.rpm                         9.7 MB/s | 213 kB     00:00    
(43/71): perl-Pod-Usage-1.69-395.el8.noarch.rpm                          1.5 MB/s |  34 kB     00:00    
(44/71): perl-Scalar-List-Utils-1.49-2.el8.x86_64.rpm                    3.4 MB/s |  68 kB     00:00    
(45/71): perl-Socket-2.027-3.el8.x86_64.rpm                              3.0 MB/s |  59 kB     00:00    
(46/71): perl-Storable-3.11-3.el8.x86_64.rpm                             4.9 MB/s |  98 kB     00:00    
(47/71): perl-Term-ANSIColor-4.06-396.el8.noarch.rpm                     2.3 MB/s |  46 kB     00:00    
(48/71): perl-Term-Cap-1.17-395.el8.noarch.rpm                           1.2 MB/s |  23 kB     00:00    
(49/71): perl-Text-ParseWords-3.30-395.el8.noarch.rpm                    938 kB/s |  18 kB     00:00    
(50/71): perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch.rpm                1.2 MB/s |  24 kB     00:00    
(51/71): perl-Time-Local-1.280-1.el8.noarch.rpm                          1.7 MB/s |  34 kB     00:00    
(52/71): perl-Unicode-Normalize-1.25-396.el8.x86_64.rpm                  3.7 MB/s |  82 kB     00:00    
(53/71): perl-constant-1.33-396.el8.noarch.rpm                           1.3 MB/s |  25 kB     00:00    
(54/71): perl-macros-5.26.3-422.el8.x86_64.rpm                           3.5 MB/s |  73 kB     00:00    
(55/71): perl-libs-5.26.3-422.el8.x86_64.rpm                              27 MB/s | 1.6 MB     00:00    
(56/71): perl-parent-0.237-1.el8.noarch.rpm                              682 kB/s |  20 kB     00:00    
(57/71): perl-podlators-4.11-1.el8.noarch.rpm                            5.3 MB/s | 118 kB     00:00    
(58/71): perl-threads-2.21-2.el8.x86_64.rpm                              2.7 MB/s |  61 kB     00:00    
(59/71): perl-threads-shared-1.58-2.el8.x86_64.rpm                       2.3 MB/s |  48 kB     00:00    
(60/71): perl-interpreter-5.26.3-422.el8.x86_64.rpm                       46 MB/s | 6.3 MB     00:00    
(61/71): nginx-mod-http-perl-1.14.1-9.module+el8.0.0+4108+af250afe.x86_6 516 kB/s |  46 kB     00:00    
(62/71): nginx-mod-http-xslt-filter-1.14.1-9.module+el8.0.0+4108+af250af 1.4 MB/s |  34 kB     00:00    
(63/71): nginx-1.14.1-9.module+el8.0.0+4108+af250afe.x86_64.rpm          3.4 MB/s | 570 kB     00:00    
(64/71): openssl-1.1.1k-9.el8_7.x86_64.rpm                               3.6 MB/s | 710 kB     00:00    
(65/71): nginx-mod-mail-1.14.1-9.module+el8.0.0+4108+af250afe.x86_64.rpm 2.0 MB/s |  64 kB     00:00    
(66/71): nginx-mod-stream-1.14.1-9.module+el8.0.0+4108+af250afe.x86_64.r 4.9 MB/s |  85 kB     00:00    
(67/71): nginx-all-modules-1.14.1-9.module+el8.0.0+4108+af250afe.noarch. 1.3 MB/s |  24 kB     00:00    
(68/71): nginx-filesystem-1.14.1-9.module+el8.0.0+4108+af250afe.noarch.r 1.3 MB/s |  24 kB     00:00    
(69/71): nginx-mod-http-image-filter-1.14.1-9.module+el8.0.0+4108+af250a 2.0 MB/s |  35 kB     00:00    
(70/71): libtiff-4.0.9-29.el8_8.x86_64.rpm                               9.3 MB/s | 189 kB     00:00    
(71/71): openssl-libs-1.1.1k-9.el8_7.x86_64.rpm                           19 MB/s | 1.5 MB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                     13 MB/s |  21 MB     00:01     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Installing       : libpng-2:1.6.34-5.el8.x86_64                                                   1/72 
  Installing       : freetype-2.9.1-9.el8.x86_64                                                    2/72 
  Installing       : fontpackages-filesystem-1.44-22.el8.noarch                                     3/72 
  Installing       : libjpeg-turbo-1.5.3-12.el8.x86_64                                              4/72 
  Installing       : dejavu-fonts-common-2.35-7.el8.noarch                                          5/72 
  Installing       : dejavu-sans-fonts-2.35-7.el8.noarch                                            6/72 
  Installing       : fontconfig-2.13.1-4.el8.x86_64                                                 7/72 
  Running scriptlet: fontconfig-2.13.1-4.el8.x86_64                                                 7/72 
  Installing       : openssl-1:1.1.1k-9.el8_7.x86_64                                                8/72 
  Upgrading        : openssl-libs-1:1.1.1k-9.el8_7.x86_64                                           9/72 
  Running scriptlet: openssl-libs-1:1.1.1k-9.el8_7.x86_64                                           9/72 
  Installing       : openssl-pkcs11-0.4.10-3.el8.x86_64                                            10/72 
  Running scriptlet: nginx-filesystem-1:1.14.1-9.module+el8.0.0+4108+af250afe.noarch               11/72 
  Installing       : nginx-filesystem-1:1.14.1-9.module+el8.0.0+4108+af250afe.noarch               11/72 
  Installing       : ncurses-6.1-9.20180224.el8.x86_64                                             12/72 
  Installing       : libxslt-1.1.32-6.el8.x86_64                                                   13/72 
  Installing       : groff-base-1.22.3-18.el8.x86_64                                               14/72 
  Installing       : perl-Digest-1.17-395.el8.noarch                                               15/72 
  Installing       : perl-Digest-MD5-2.55-396.el8.x86_64                                           16/72 
  Installing       : perl-Data-Dumper-2.167-399.el8.x86_64                                         17/72 
  Installing       : perl-libnet-3.11-3.el8.noarch                                                 18/72 
  Installing       : perl-URI-1.73-3.el8.noarch                                                    19/72 
  Installing       : perl-Pod-Escapes-1:1.07-395.el8.noarch                                        20/72 
  Installing       : perl-Net-SSLeay-1.88-2.module_el8+339+1ec643e0.x86_64                         21/72 
  Installing       : perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch                     22/72 
  Installing       : perl-IO-Socket-IP-0.39-5.el8.noarch                                           23/72 
  Installing       : perl-Time-Local-1:1.280-1.el8.noarch                                          24/72 
  Installing       : perl-IO-Socket-SSL-2.066-4.module_el8+339+1ec643e0.noarch                     25/72 
  Installing       : perl-Term-ANSIColor-4.06-396.el8.noarch                                       26/72 
  Installing       : perl-Term-Cap-1.17-395.el8.noarch                                             27/72 
  Installing       : perl-File-Temp-0.230.600-1.el8.noarch                                         28/72 
  Installing       : perl-Pod-Simple-1:3.35-395.el8.noarch                                         29/72 
  Installing       : perl-HTTP-Tiny-0.074-2.el8.noarch                                             30/72 
  Installing       : perl-podlators-4.11-1.el8.noarch                                              31/72 
  Installing       : perl-Pod-Perldoc-3.28-396.el8.noarch                                          32/72 
  Installing       : perl-Text-ParseWords-3.30-395.el8.noarch                                      33/72 
  Installing       : perl-Pod-Usage-4:1.69-395.el8.noarch                                          34/72 
  Installing       : perl-MIME-Base64-3.15-396.el8.x86_64                                          35/72 
  Installing       : perl-Storable-1:3.11-3.el8.x86_64                                             36/72 
  Installing       : perl-Getopt-Long-1:2.50-4.el8.noarch                                          37/72 
  Installing       : perl-Errno-1.28-422.el8.x86_64                                                38/72 
  Installing       : perl-Socket-4:2.027-3.el8.x86_64                                              39/72 
  Installing       : perl-Encode-4:2.97-3.el8.x86_64                                               40/72 
  Installing       : perl-Carp-1.42-396.el8.noarch                                                 41/72 
  Installing       : perl-Exporter-5.72-396.el8.noarch                                             42/72 
  Installing       : perl-libs-4:5.26.3-422.el8.x86_64                                             43/72 
  Installing       : perl-Scalar-List-Utils-3:1.49-2.el8.x86_64                                    44/72 
  Installing       : perl-parent-1:0.237-1.el8.noarch                                              45/72 
  Installing       : perl-macros-4:5.26.3-422.el8.x86_64                                           46/72 
  Installing       : perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch                                  47/72 
  Installing       : perl-Unicode-Normalize-1.25-396.el8.x86_64                                    48/72 
  Installing       : perl-File-Path-2.15-2.el8.noarch                                              49/72 
  Installing       : perl-IO-1.38-422.el8.x86_64                                                   50/72 
  Installing       : perl-PathTools-3.74-1.el8.x86_64                                              51/72 
  Installing       : perl-constant-1.33-396.el8.noarch                                             52/72 
  Installing       : perl-threads-1:2.21-2.el8.x86_64                                              53/72 
  Installing       : perl-threads-shared-1.58-2.el8.x86_64                                         54/72 
  Installing       : perl-interpreter-4:5.26.3-422.el8.x86_64                                      55/72 
  Installing       : libwebp-1.0.0-9.el8.x86_64                                                    56/72 
  Installing       : libXau-1.0.9-3.el8.x86_64                                                     57/72 
  Installing       : libxcb-1.13.1-1.el8.x86_64                                                    58/72 
  Installing       : libX11-common-1.6.8-7.el8.noarch                                              59/72 
  Installing       : libX11-1.6.8-7.el8.x86_64                                                     60/72 
  Installing       : libXpm-3.5.12-11.el8.x86_64                                                   61/72 
  Installing       : jbigkit-libs-2.1-14.el8.x86_64                                                62/72 
  Running scriptlet: jbigkit-libs-2.1-14.el8.x86_64                                                62/72 
  Installing       : libtiff-4.0.9-29.el8_8.x86_64                                                 63/72 
  Installing       : gd-2.2.5-7.el8.x86_64                                                         64/72 
  Running scriptlet: gd-2.2.5-7.el8.x86_64                                                         64/72 
  Installing       : nginx-mod-http-perl-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64            65/72 
  Running scriptlet: nginx-mod-http-perl-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64            65/72 
  Installing       : nginx-mod-http-xslt-filter-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64     66/72 
  Running scriptlet: nginx-mod-http-xslt-filter-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64     66/72 
  Installing       : nginx-mod-mail-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                 67/72 
  Running scriptlet: nginx-mod-mail-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                 67/72 
  Installing       : nginx-mod-stream-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64               68/72 
  Running scriptlet: nginx-mod-stream-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64               68/72 
  Installing       : nginx-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                          69/72 
  Running scriptlet: nginx-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                          69/72 
  Installing       : nginx-mod-http-image-filter-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64    70/72 
  Running scriptlet: nginx-mod-http-image-filter-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64    70/72 
  Installing       : nginx-all-modules-1:1.14.1-9.module+el8.0.0+4108+af250afe.noarch              71/72 
  Cleanup          : openssl-libs-1:1.1.1k-7.el8_6.x86_64                                          72/72 
  Running scriptlet: openssl-libs-1:1.1.1k-7.el8_6.x86_64                                          72/72 
  Running scriptlet: fontconfig-2.13.1-4.el8.x86_64                                                72/72 
  Verifying        : gd-2.2.5-7.el8.x86_64                                                          1/72 
  Verifying        : jbigkit-libs-2.1-14.el8.x86_64                                                 2/72 
  Verifying        : libX11-1.6.8-7.el8.x86_64                                                      3/72 
  Verifying        : libX11-common-1.6.8-7.el8.noarch                                               4/72 
  Verifying        : libXau-1.0.9-3.el8.x86_64                                                      5/72 
  Verifying        : libXpm-3.5.12-11.el8.x86_64                                                    6/72 
  Verifying        : libjpeg-turbo-1.5.3-12.el8.x86_64                                              7/72 
  Verifying        : libwebp-1.0.0-9.el8.x86_64                                                     8/72 
  Verifying        : libxcb-1.13.1-1.el8.x86_64                                                     9/72 
  Verifying        : perl-Digest-1.17-395.el8.noarch                                               10/72 
  Verifying        : perl-Digest-MD5-2.55-396.el8.x86_64                                           11/72 
  Verifying        : perl-IO-Socket-IP-0.39-5.el8.noarch                                           12/72 
  Verifying        : perl-IO-Socket-SSL-2.066-4.module_el8+339+1ec643e0.noarch                     13/72 
  Verifying        : perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch                     14/72 
  Verifying        : perl-Net-SSLeay-1.88-2.module_el8+339+1ec643e0.x86_64                         15/72 
  Verifying        : perl-URI-1.73-3.el8.noarch                                                    16/72 
  Verifying        : perl-libnet-3.11-3.el8.noarch                                                 17/72 
  Verifying        : dejavu-fonts-common-2.35-7.el8.noarch                                         18/72 
  Verifying        : dejavu-sans-fonts-2.35-7.el8.noarch                                           19/72 
  Verifying        : fontconfig-2.13.1-4.el8.x86_64                                                20/72 
  Verifying        : fontpackages-filesystem-1.44-22.el8.noarch                                    21/72 
  Verifying        : freetype-2.9.1-9.el8.x86_64                                                   22/72 
  Verifying        : groff-base-1.22.3-18.el8.x86_64                                               23/72 
  Verifying        : libpng-2:1.6.34-5.el8.x86_64                                                  24/72 
  Verifying        : libxslt-1.1.32-6.el8.x86_64                                                   25/72 
  Verifying        : ncurses-6.1-9.20180224.el8.x86_64                                             26/72 
  Verifying        : openssl-pkcs11-0.4.10-3.el8.x86_64                                            27/72 
  Verifying        : perl-Carp-1.42-396.el8.noarch                                                 28/72 
  Verifying        : perl-Data-Dumper-2.167-399.el8.x86_64                                         29/72 
  Verifying        : perl-Encode-4:2.97-3.el8.x86_64                                               30/72 
  Verifying        : perl-Errno-1.28-422.el8.x86_64                                                31/72 
  Verifying        : perl-Exporter-5.72-396.el8.noarch                                             32/72 
  Verifying        : perl-File-Path-2.15-2.el8.noarch                                              33/72 
  Verifying        : perl-File-Temp-0.230.600-1.el8.noarch                                         34/72 
  Verifying        : perl-Getopt-Long-1:2.50-4.el8.noarch                                          35/72 
  Verifying        : perl-HTTP-Tiny-0.074-2.el8.noarch                                             36/72 
  Verifying        : perl-IO-1.38-422.el8.x86_64                                                   37/72 
  Verifying        : perl-MIME-Base64-3.15-396.el8.x86_64                                          38/72 
  Verifying        : perl-PathTools-3.74-1.el8.x86_64                                              39/72 
  Verifying        : perl-Pod-Escapes-1:1.07-395.el8.noarch                                        40/72 
  Verifying        : perl-Pod-Perldoc-3.28-396.el8.noarch                                          41/72 
  Verifying        : perl-Pod-Simple-1:3.35-395.el8.noarch                                         42/72 
  Verifying        : perl-Pod-Usage-4:1.69-395.el8.noarch                                          43/72 
  Verifying        : perl-Scalar-List-Utils-3:1.49-2.el8.x86_64                                    44/72 
  Verifying        : perl-Socket-4:2.027-3.el8.x86_64                                              45/72 
  Verifying        : perl-Storable-1:3.11-3.el8.x86_64                                             46/72 
  Verifying        : perl-Term-ANSIColor-4.06-396.el8.noarch                                       47/72 
  Verifying        : perl-Term-Cap-1.17-395.el8.noarch                                             48/72 
  Verifying        : perl-Text-ParseWords-3.30-395.el8.noarch                                      49/72 
  Verifying        : perl-Text-Tabs+Wrap-2013.0523-395.el8.noarch                                  50/72 
  Verifying        : perl-Time-Local-1:1.280-1.el8.noarch                                          51/72 
  Verifying        : perl-Unicode-Normalize-1.25-396.el8.x86_64                                    52/72 
  Verifying        : perl-constant-1.33-396.el8.noarch                                             53/72 
  Verifying        : perl-interpreter-4:5.26.3-422.el8.x86_64                                      54/72 
  Verifying        : perl-libs-4:5.26.3-422.el8.x86_64                                             55/72 
  Verifying        : perl-macros-4:5.26.3-422.el8.x86_64                                           56/72 
  Verifying        : perl-parent-1:0.237-1.el8.noarch                                              57/72 
  Verifying        : perl-podlators-4.11-1.el8.noarch                                              58/72 
  Verifying        : perl-threads-1:2.21-2.el8.x86_64                                              59/72 
  Verifying        : perl-threads-shared-1.58-2.el8.x86_64                                         60/72 
  Verifying        : openssl-1:1.1.1k-9.el8_7.x86_64                                               61/72 
  Verifying        : nginx-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                          62/72 
  Verifying        : nginx-mod-http-perl-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64            63/72 
  Verifying        : nginx-mod-http-xslt-filter-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64     64/72 
  Verifying        : nginx-mod-mail-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                 65/72 
  Verifying        : nginx-mod-stream-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64               66/72 
  Verifying        : nginx-all-modules-1:1.14.1-9.module+el8.0.0+4108+af250afe.noarch              67/72 
  Verifying        : nginx-filesystem-1:1.14.1-9.module+el8.0.0+4108+af250afe.noarch               68/72 
  Verifying        : nginx-mod-http-image-filter-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64    69/72 
  Verifying        : libtiff-4.0.9-29.el8_8.x86_64                                                 70/72 
  Verifying        : openssl-libs-1:1.1.1k-9.el8_7.x86_64                                          71/72 
  Verifying        : openssl-libs-1:1.1.1k-7.el8_6.x86_64                                          72/72 
Installed products updated.

Upgraded:
  openssl-libs-1:1.1.1k-9.el8_7.x86_64                                                                   
Installed:
  dejavu-fonts-common-2.35-7.el8.noarch                                                                  
  dejavu-sans-fonts-2.35-7.el8.noarch                                                                    
  fontconfig-2.13.1-4.el8.x86_64                                                                         
  fontpackages-filesystem-1.44-22.el8.noarch                                                             
  freetype-2.9.1-9.el8.x86_64                                                                            
  gd-2.2.5-7.el8.x86_64                                                                                  
  groff-base-1.22.3-18.el8.x86_64                                                                        
  jbigkit-libs-2.1-14.el8.x86_64                                                                         
  libX11-1.6.8-7.el8.x86_64                                                                              
  libX11-common-1.6.8-7.el8.noarch                                                                       
  libXau-1.0.9-3.el8.x86_64                                                                              
  libXpm-3.5.12-11.el8.x86_64                                                                            
  libjpeg-turbo-1.5.3-12.el8.x86_64                                                                      
  libpng-2:1.6.34-5.el8.x86_64                                                                           
  libtiff-4.0.9-29.el8_8.x86_64                                                                          
  libwebp-1.0.0-9.el8.x86_64                                                                             
  libxcb-1.13.1-1.el8.x86_64                                                                             
  libxslt-1.1.32-6.el8.x86_64                                                                            
  ncurses-6.1-9.20180224.el8.x86_64                                                                      
  nginx-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                                                   
  nginx-all-modules-1:1.14.1-9.module+el8.0.0+4108+af250afe.noarch                                       
  nginx-filesystem-1:1.14.1-9.module+el8.0.0+4108+af250afe.noarch                                        
  nginx-mod-http-image-filter-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                             
  nginx-mod-http-perl-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                                     
  nginx-mod-http-xslt-filter-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                              
  nginx-mod-mail-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                                          
  nginx-mod-stream-1:1.14.1-9.module+el8.0.0+4108+af250afe.x86_64                                        
  openssl-1:1.1.1k-9.el8_7.x86_64                                                                        
  openssl-pkcs11-0.4.10-3.el8.x86_64                                                                     
  perl-Carp-1.42-396.el8.noarch                                                                          
  perl-Data-Dumper-2.167-399.el8.x86_64                                                                  
  perl-Digest-1.17-395.el8.noarch                                                                        
  perl-Digest-MD5-2.55-396.el8.x86_64                                                                    
  perl-Encode-4:2.97-3.el8.x86_64                                                                        
  perl-Errno-1.28-422.el8.x86_64                                                                         
  perl-Exporter-5.72-396.el8.noarch                                                                      
  perl-File-Path-2.15-2.el8.noarch                                                                       
  perl-File-Temp-0.230.600-1.el8.noarch                                                                  
  perl-Getopt-Long-1:2.50-4.el8.noarch                                                                   
  perl-HTTP-Tiny-0.074-2.el8.noarch                                                                      
  perl-IO-1.38-422.el8.x86_64                                                                            
  perl-IO-Socket-IP-0.39-5.el8.noarch                                                                    
  perl-IO-Socket-SSL-2.066-4.module_el8+339+1ec643e0.noarch                                              
  perl-MIME-Base64-3.15-396.el8.x86_64                                                                   
  perl-Mozilla-CA-20160104-7.module_el8+645+9d809f8c.noarch                                              
  perl-Net-SSLeay-1.88-2.module_el8+339+1ec643e0.x86_64                                                  
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
  perl-URI-1.73-3.el8.noarch                                                                             
  perl-Unicode-Normalize-1.25-396.el8.x86_64                                                             
  perl-constant-1.33-396.el8.noarch                                                                      
  perl-interpreter-4:5.26.3-422.el8.x86_64                                                               
  perl-libnet-3.11-3.el8.noarch                                                                          
  perl-libs-4:5.26.3-422.el8.x86_64                                                                      
  perl-macros-4:5.26.3-422.el8.x86_64                                                                    
  perl-parent-1:0.237-1.el8.noarch                                                                       
  perl-podlators-4.11-1.el8.noarch                                                                       
  perl-threads-1:2.21-2.el8.x86_64                                                                       
  perl-threads-shared-1.58-2.el8.x86_64                                                                  

Complete!
[root@stapp03 ~]# 
```

**4. For installing php-fpm install the php repo**

```
 yum install http://rpms.remirepo.net/enterprise/remi-release-8.rpm
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 0:01:19 ago on Sat Nov 11 19:41:56 2023.
remi-release-8.rpm                                                        86 kB/s |  32 kB     00:00    
Dependencies resolved.
=========================================================================================================
 Package                  Architecture       Version                      Repository                Size
=========================================================================================================
Installing:
 remi-release             noarch             8.8-1.el8.remi               @commandline              32 k

Transaction Summary
=========================================================================================================
Install  1 Package

Total size: 32 k
Installed size: 27 k
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Installing       : remi-release-8.8-1.el8.remi.noarch                                              1/1 
  Verifying        : remi-release-8.8-1.el8.remi.noarch                                              1/1 
Installed products updated.

Installed:
  remi-release-8.8-1.el8.remi.noarch                                                                     

Complete!
[root@stapp01 ~]# 
```

**5. Disable the default PHP-fpm version repo**

```
[root@stapp03 ~]# yum-config-manager --disable remi-php54
-bash: yum-config-manager: command not found
[root@stapp03 ~]# yum install yum-utils
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Remi's Modular repository for Enterprise Linux 8 - x86_64                859  B/s | 833  B     00:00    
Remi's Modular repository for Enterprise Linux 8 - x86_64                3.0 MB/s | 3.1 kB     00:00    
Importing GPG key 0x5F11735A:
 Userid     : "Remi's RPM repository <remi@remirepo.net>"
 Fingerprint: 6B38 FEA7 231F 87F5 2B9C A9D8 5550 9759 5F11 735A
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-remi.el8
Is this ok [y/N]: y
Remi's Modular repository for Enterprise Linux 8 - x86_64                778 kB/s | 1.4 MB     00:01    
Safe Remi's RPM repository for Enterprise Linux 8 - x86_64               1.1 kB/s | 833  B     00:00    
Safe Remi's RPM repository for Enterprise Linux 8 - x86_64               3.0 MB/s | 3.1 kB     00:00    
Importing GPG key 0x5F11735A:
 Userid     : "Remi's RPM repository <remi@remirepo.net>"
 Fingerprint: 6B38 FEA7 231F 87F5 2B9C A9D8 5550 9759 5F11 735A
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-remi.el8
Is this ok [y/N]: y
Safe Remi's RPM repository for Enterprise Linux 8 - x86_64               1.9 MB/s | 2.7 MB     00:01    
Dependencies resolved.
=========================================================================================================
 Package                             Architecture      Version                   Repository         Size
=========================================================================================================
Installing:
 yum-utils                           noarch            4.0.21-23.el8             baseos             80 k
Upgrading:
 python3-dnf-plugins-core            noarch            4.0.21-23.el8             baseos            279 k
Installing dependencies:
 dnf-plugins-core                    noarch            4.0.21-23.el8             baseos             77 k

Transaction Summary
=========================================================================================================
Install  2 Packages
Upgrade  1 Package

Total download size: 436 k
Is this ok [y/N]: y
Downloading Packages:
(1/3): yum-utils-4.0.21-23.el8.noarch.rpm                                909 kB/s |  80 kB     00:00    
(2/3): dnf-plugins-core-4.0.21-23.el8.noarch.rpm                         848 kB/s |  77 kB     00:00    
(3/3): python3-dnf-plugins-core-4.0.21-23.el8.noarch.rpm                 2.2 MB/s | 279 kB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                    2.3 MB/s | 436 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Running scriptlet: python3-dnf-plugins-core-4.0.21-23.el8.noarch                                   1/1 
  Upgrading        : python3-dnf-plugins-core-4.0.21-23.el8.noarch                                   1/4 
  Installing       : dnf-plugins-core-4.0.21-23.el8.noarch                                           2/4 
  Installing       : yum-utils-4.0.21-23.el8.noarch                                                  3/4 
  Cleanup          : python3-dnf-plugins-core-4.0.21-18.el8.noarch                                   4/4 
  Running scriptlet: python3-dnf-plugins-core-4.0.21-18.el8.noarch                                   4/4 
  Verifying        : dnf-plugins-core-4.0.21-23.el8.noarch                                           1/4 
  Verifying        : yum-utils-4.0.21-23.el8.noarch                                                  2/4 
  Verifying        : python3-dnf-plugins-core-4.0.21-23.el8.noarch                                   3/4 
  Verifying        : python3-dnf-plugins-core-4.0.21-18.el8.noarch                                   4/4 
Installed products updated.

Upgraded:
  python3-dnf-plugins-core-4.0.21-23.el8.noarch                                                          
Installed:
  dnf-plugins-core-4.0.21-23.el8.noarch                  yum-utils-4.0.21-23.el8.noarch                 

Complete!
[root@stapp03 ~]# 
```

**6. For installing php-fpm**


```
[root@stapp01 ~]# yum install php-fpm
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 0:11:59 ago on Sat Nov 11 19:46:21 2023.
Dependencies resolved.
=========================================================================================================
 Package             Arch      Version                                     Repository               Size
=========================================================================================================
Installing:
 php-fpm             x86_64    7.2.24-1.module+el8.2.0+4601+7c76a223       ubi-8-appstream-rpms    1.6 M
Installing dependencies:
 httpd-filesystem    noarch    2.4.37-62.module_el8+657+88b2113f           appstream                44 k
 php-common          x86_64    7.2.24-1.module+el8.2.0+4601+7c76a223       ubi-8-appstream-rpms    662 k
Enabling module streams:
 httpd                         2.4                                                                      
 php                           7.2                                                                      

Transaction Summary
=========================================================================================================
Install  3 Packages

Total download size: 2.3 M
Installed size: 11 M
Is this ok [y/N]: y
Downloading Packages:
(1/3): php-common-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64.rpm       2.2 MB/s | 662 kB     00:00    
(2/3): httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch.rpm     138 kB/s |  44 kB     00:00    
(3/3): php-fpm-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64.rpm          4.6 MB/s | 1.6 MB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                    3.5 MB/s | 2.3 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                 1/1 
  Installing       : php-common-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64                         1/3 
  Running scriptlet: httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                       2/3 
  Installing       : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                       2/3 
  Installing       : php-fpm-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64                            3/3 
  Running scriptlet: php-fpm-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64                            3/3 
  Verifying        : httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                       1/3 
  Verifying        : php-common-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64                         2/3 
  Verifying        : php-fpm-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64                            3/3 
Installed products updated.

Installed:
  httpd-filesystem-2.4.37-62.module_el8+657+88b2113f.noarch                                              
  php-common-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64                                                
  php-fpm-7.2.24-1.module+el8.2.0+4601+7c76a223.x86_64                                                   

Complete!
[root@stapp01 ~]# 
```


**7. Edit the Nginx configuration**

```
[root@stapp01 ~]# vi /etc/nginx/nginx.conf
[root@stapp01 ~]# cat /etc/nginx/nginx.conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       8096 default_server;
        listen       [::]:8096 default_server;
        server_name  _;
        root         /var/www/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers PROFILE=SYSTEM;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

}

[root@stapp01 ~]# 
```

**8.  change the user and group- nginx**

```
[root@stapp01 ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
[root@stapp01 ~]# vi /etc/php-fpm.d/www.conf
change the user = nginx
group = nginx

[root@stapp01 ~]# ls /var/www/html
index.php  info.php

```
**9. run the nginx start & status**

```
[root@stapp01 ~]# ls -lsd
8 dr-xr-x--- 1 root root 4096 Feb  7  2023 .
[root@stapp01 ~]# ls /var/www/html
index.php  info.php

[root@stapp01 ~]# systemctl start nginx
[root@stapp01 ~]# systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
  Drop-In: /usr/lib/systemd/system/nginx.service.d
           └─php-fpm.conf
   Active: active (running) since Sat 2023-11-11 20:10:37 UTC; 7s ago
  Process: 3109 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
  Process: 3083 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Process: 3070 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
 Main PID: 3127 (nginx)
    Tasks: 37 (limit: 1340692)
   Memory: 54.3M
   CGroup: /docker/be26cec2e1005d48d50cc696a34ec7ae7ec46f1ac64457d972c64dafe0bf3cea/system.slice/nginx.se
rvice
           ├─3127 nginx: master process /usr/sbin/nginx
           ├─3128 nginx: worker process
           ├─3129 nginx: worker process
           ├─3130 nginx: worker process
           ├─3131 nginx: worker process
           ├─3132 nginx: worker process
           ├─3133 nginx: worker process
           ├─3134 nginx: worker process
           ├─3135 nginx: worker process
           ├─3136 nginx: worker process
           ├─3137 nginx: worker process
           ├─3138 nginx: worker process
           ├─3139 nginx: worker process
           ├─3140 nginx: worker process
           ├─3141 nginx: worker process
           ├─3142 nginx: worker process
           ├─3143 nginx: worker process
           ├─3144 nginx: worker process
           ├─3145 nginx: worker process
           ├─3146 nginx: worker process
           ├─3147 nginx: worker process
           ├─3148 nginx: worker process
           ├─3149 nginx: worker process
           ├─3150 nginx: worker process
           ├─3151 nginx: worker process
           ├─3152 nginx: worker process
           ├─3153 nginx: worker process
           ├─3154 nginx: worker process
           ├─3155 nginx: worker process
           ├─3156 nginx: worker process
           ├─3157 nginx: worker process
           ├─3158 nginx: worker process
           ├─3159 nginx: worker process
           ├─3160 nginx: worker process
           ├─3161 nginx: worker process
           ├─3162 nginx: worker process
           └─3163 nginx: worker process

Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[3109]: nginx.service: Exe
cuting: /usr/sbin/nginx
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Child 
3109 belongs to nginx.service.
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Contro
l process exited, code=exited status=0
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Got fi
nal SIGCHLD for state start.
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: New ma
in PID 3127 belongs to service, we are happy.
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Main P
ID loaded: 3127
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Change
d start -> running
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Job ng
inx.service/start finished, result=done
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: Started The nginx HTTP and reverse proxy serv
er.
Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: nginx.service: Failed
 to send unit change signal for nginx.service: Connection reset by peer
[root@stapp01 ~]# 
```

**10. Run the php-fpm start & status**

```
[root@stapp01 ~]# systemctl start php-fpm
[root@stapp01 ~]# systemctl status php-fpm
● php-fpm.service - The PHP FastCGI Process Manager
   Loaded: loaded (/usr/lib/systemd/system/php-fpm.service; disabled; vendor preset: disabled)
   Active: active (running) since Sat 2023-11-11 20:10:37 UTC; 52s ago
 Main PID: 3076 (php-fpm)
   Status: "Processes active: 0, idle: 5, Requests: 0, slow: 0, Traffic: 0req/sec"
    Tasks: 6 (limit: 1340692)
   Memory: 8.8M
   CGroup: /docker/be26cec2e1005d48d50cc696a34ec7ae7ec46f1ac64457d972c64dafe0bf3cea/system.slice/php-fpm.
service
           ├─3076 php-fpm: master process (/etc/php-fpm.conf)
           ├─3116 php-fpm: pool www
           ├─3117 php-fpm: pool www
           ├─3118 php-fpm: pool www
           ├─3119 php-fpm: pool www
           └─3120 php-fpm: pool www

Nov 11 20:10:37 stapp01.stratos.xfusioncorp.com systemd[1]: Started The PHP FastCGI Process Manager.
Nov 11 20:10:47 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Got 
notification message from PID 3076 (READY=1, STATUS=Processes active: 0, idle: 5, Requests: 0, slow: 0, T
raffic: 0req/sec)
Nov 11 20:10:57 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Got 
notification message from PID 3076 (READY=1, STATUS=Processes active: 0, idle: 5, Requests: 0, slow: 0, T
raffic: 0req/sec)
Nov 11 20:11:07 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Got 
notification message from PID 3076 (READY=1, STATUS=Processes active: 0, idle: 5, Requests: 0, slow: 0, T
raffic: 0req/sec)
Nov 11 20:11:17 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Got 
notification message from PID 3076 (READY=1, STATUS=Processes active: 0, idle: 5, Requests: 0, slow: 0, T
raffic: 0req/sec)
Nov 11 20:11:19 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Tryi
ng to enqueue job php-fpm.service/start/replace
Nov 11 20:11:19 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Inst
alled new job php-fpm.service/start as 130
Nov 11 20:11:19 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Enqu
eued job php-fpm.service/start as 130
Nov 11 20:11:19 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Job 
php-fpm.service/start finished, result=done
Nov 11 20:11:27 stapp01.stratos.xfusioncorp.com systemd[1]: php-fpm.service: Got 
notification message from PID 3076 (READY=1, STATUS=Processes active: 0, idle: 5, Requests: 0, slow: 0, T
raffic: 0req/sec)

[root@stapp01 ~]# curl http://stapp01:8096/index.php
Welcome to xFusionCorp Industries![root@stapp01 ~]# 
```

**11.  validate the task by curl**
```
[root@stapp01 ~]# curl -i http://stapp01:8096
HTTP/1.1 200 OK
Server: nginx/1.14.1
Date: Sat, 11 Nov 2023 20:14:01 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
X-Powered-By: PHP/7.2.24

Welcome to xFusionCorp Industries!
[root@stapp01 ~]# 