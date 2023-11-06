

## Question

The Nautilus application development team is working on to develop a Node app. They are still in the development phase however they want to deploy and test their app on a containerized environment and using a Jenkins pipeline. Please find below more details to complete this task.

  

Click on the  `Jenkins`  button on the top bar to access the Jenkins UI. Login using username  `admin`  and password  `Adm!n321`.  
  

Similarly, click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`.  
  
There is a repository named  `sarah/web`  in Gitea, which is cloned on the  `Storage`  server under  `/home/sarah/web`  directory.  
  

-   A  `Dockerfile`  is already present under the git repository, please push the same to the origin repo if not pushed already.  
      
    
    -   Create a jenkins pipeline job named  `node-app`  and configure it as below:  
          
        
    -   Configure it to deploy the app on  `App Server 3`
            
    -   The pipeline must have two stages  `Build`  and  `Deploy`  (names are case sensitive)  
              
            
     -   In the  `Build`  stage, build an image named  `stregi01.stratos.xfusioncorp.com:5000/node-app:latest`  using the  `Dockerfile`  present under the Git repository.  `stregi01.stratos.xfusioncorp.com:5000`  is the image registry server. After building the image push the same to the image registry server.  
              
            
        -   In the  `Deploy`  stage, create a container named  `node-app`  using the image you build it the  `Build`  stage. Make sure to map the container port with host port  `8080`.  
              
            

`Note:`  
  

1.  You might need to install some plugins and restart Jenkins service. So, we recommend clicking on  `Restart Jenkins when installation is complete and no jobs are running`  on plugin installation/update page i.e  `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.  
      
    
2.  For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.




## Solution

1.  **Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8081 and click on Display Port. You should be able to access the Jenkins login page. Login using username the  `admin`  and  `Adm!n321`  password.**

![1](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/6b896eea-6413-48a5-8722-e5ec184540e4)

![2](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/2bcd5970-26fb-4578-b270-8dd2416bfbfd)


**2. Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.**

![3](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/96f47695-1f74-4f44-8fb5-ac1428bc77c5)

- Search for ssh , credentials, pipeline & git -> select all plugin checkbox click and click Download now and install after restart.

![4](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/329d5774-ea38-4a01-83e4-21800e216a03)

-   click Download now and install after restart.

![5](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c7b26f58-d464-4195-8513-6f7e2c0bc4f3)

![6](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/7b837a51-21a8-487a-afcf-613a97d80e3b)

-   Restart jenkins

3.  **Click jenkins > manage jenkins > Credentials**

![7](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4f8f97a5-7750-4139-ba42-23fd215570f5)


-   Click credentials
    
-   Click jenkins > manage jenkins > Credentials > System

![8](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/853533bc-f21e-4171-8f71-15c0588b0c7b)

- Click jenkins > manage jenkins > Credentials > System > Global credentials(unrestricted)

![9](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/4444965b-a17f-4692-bffe-6b0d7f5adfab)

- Click add Credentials button

  -   kind -> Username with password Username -> banner password -> BigGr33n ID -> banner

![10](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/109622f5-3861-4d4b-ac31-df508f33f0f1)

- Click create.

![11](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/5ed21f88-143e-4935-92ce-13bd2a7bd739)

**4.  Add Manage node and cloud as per the task**

- Click jenkins > manage jenkins > Manage Nodes and clouds >


![12](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ab9e3890-81d7-443d-96e3-e25d406d8803)

- Click New Node button

![13](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/44299b19-92ca-4f4a-a4cb-1acc3a10ae0a)

-   Click node
    
-   Node name -> app03
    
-   Type -> Permanent agent


![14](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/f0613d8f-4a2e-4d3f-96f0-112f6f64cb90)


-   Click create

```

Name -> app03

Number of executors -> 1

Remote root directory - /home/banner

labels : app03

launch method : only build jobs with label expression matching this node

Credentials -> choose banner

Host key verification strategy -> manually trusted key verification strategy

```

![15](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e080bae8-8f59-4b85-adf0-5436ef5d9625)

-   Click save.
    
-   Create node server


![16](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/8b07bfdf-bc09-4473-9032-d119892f8563)


-   Back to session terminal.

```

thor@jump_host ~$ ssh banner@stapp03
The authenticity of host 'stapp03 (172.16.238.12)' can't be established.
ECDSA key fingerprint is SHA256:Lq3Ay0MccZq4yQS4ZOHS5nuq8rNv/gxt3nwjgniP74w.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03,172.16.238.12' (ECDSA) to the list of known hosts.
banner@stapp03's password: BigGr33n
[banner@stapp03 ~]$ sudo su 

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: BigGr33n
[root@stapp03 banner]#
```
- Install java-17
```
[root@stapp03 banner]# yum install java-17-openjdk -y
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

CentOS Stream 8 - AppStream                                               22 kB/s | 4.4 kB     00:00    
CentOS Stream 8 - AppStream                                               30 MB/s |  34 MB     00:01    
CentOS Stream 8 - BaseOS                                                  35 kB/s | 3.9 kB     00:00    
CentOS Stream 8 - BaseOS                                                  90 MB/s |  52 MB     00:00    
CentOS Stream 8 - Extras                                                  19 kB/s | 2.9 kB     00:00    
CentOS Stream 8 - Extras common packages                                  21 kB/s | 3.0 kB     00:00    
CentOS Stream 8 - Extras common packages                                  29 kB/s | 6.9 kB     00:00    
Docker CE Stable - x86_64                                                 40 kB/s | 3.5 kB     00:00    
Docker CE Stable - x86_64                                                497 kB/s |  51 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - BaseOS                           1.1 MB/s | 716 kB     00:00    
Red Hat Universal Base Image 8 (RPMs) - AppStream                         15 MB/s | 2.9 MB     00:00    
Red Hat Universal Base Image 8 (RPMs) - CodeReady Builder                780 kB/s |  99 kB     00:00    
Dependencies resolved.
=========================================================================================================
 Package                     Arch     Version                               Repository              Size
=========================================================================================================
Installing:
 java-17-openjdk             x86_64   1:17.0.9.0.9-2.el8                    ubi-8-appstream-rpms   458 k
Installing dependencies:
 adwaita-cursor-theme        noarch   3.28.0-3.el8                          appstream              647 k
 adwaita-icon-theme          noarch   3.28.0-3.el8                          appstream               11 M
 alsa-lib                    x86_64   1.2.9-1.el8                           appstream              515 k
 at-spi2-atk                 x86_64   2.26.2-1.el8                          appstream               89 k
 at-spi2-core                x86_64   2.28.0-1.el8                          appstream              169 k
 atk                         x86_64   2.28.1-1.el8                          appstream              272 k
 avahi-libs                  x86_64   0.7-21.el8                            baseos                  62 k
 cairo                       x86_64   1.15.12-6.el8                         appstream              719 k
 cairo-gobject               x86_64   1.15.12-6.el8                         appstream               33 k
 colord-libs                 x86_64   1.4.2-1.el8                           appstream              236 k
 copy-jdk-configs            noarch   4.0-2.el8                             appstream               31 k
 cups-libs                   x86_64   1:2.2.6-54.el8                        baseos                 436 k
 dejavu-fonts-common         noarch   2.35-7.el8                            baseos                  74 k
 dejavu-sans-mono-fonts      noarch   2.35-7.el8                            baseos                 447 k
 fontconfig                  x86_64   2.13.1-4.el8                          baseos                 274 k
 fontpackages-filesystem     noarch   1.44-22.el8                           baseos                  16 k
 freetype                    x86_64   2.9.1-9.el8                           baseos                 394 k
 fribidi                     x86_64   1.0.4-9.el8                           appstream               89 k
 gdk-pixbuf2                 x86_64   2.36.12-5.el8                         baseos                 467 k
 gdk-pixbuf2-modules         x86_64   2.36.12-5.el8                         appstream              109 k
 glib-networking             x86_64   2.56.1-1.1.el8                        baseos                 155 k
 graphite2                   x86_64   1.3.10-10.el8                         appstream              122 k
 gsettings-desktop-schemas   x86_64   3.32.0-6.el8                          baseos                 633 k
 gtk-update-icon-cache       x86_64   3.22.30-11.el8                        appstream               32 k
 harfbuzz                    x86_64   1.7.5-4.el8                           appstream              296 k
 hicolor-icon-theme          noarch   0.17-2.el8                            appstream               49 k
 jasper-libs                 x86_64   2.0.14-5.el8                          appstream              167 k
 java-17-openjdk-headless    x86_64   1:17.0.9.0.9-2.el8                    ubi-8-appstream-rpms    46 M
 javapackages-filesystem     noarch   5.3.0-1.module+el8+2447+6f56d9a6      ubi-8-appstream-rpms    30 k
 jbigkit-libs                x86_64   2.1-14.el8                            appstream               55 k
 lcms2                       x86_64   2.9-2.el8                             appstream              165 k
 libX11                      x86_64   1.6.8-7.el8                           appstream              612 k
 libX11-common               noarch   1.6.8-7.el8                           appstream              211 k
 libXau                      x86_64   1.0.9-3.el8                           appstream               37 k
 libXcomposite               x86_64   0.4.4-14.el8                          appstream               28 k
 libXcursor                  x86_64   1.1.15-3.el8                          appstream               36 k
 libXdamage                  x86_64   1.1.4-14.el8                          appstream               27 k
 libXext                     x86_64   1.3.4-1.el8                           appstream               45 k
 libXfixes                   x86_64   5.0.3-7.el8                           appstream               25 k
 libXft                      x86_64   2.3.3-1.el8                           appstream               67 k
 libXi                       x86_64   1.7.10-1.el8                          appstream               49 k
 libXinerama                 x86_64   1.1.4-1.el8                           appstream               16 k
 libXrandr                   x86_64   1.5.2-1.el8                           appstream               34 k
 libXrender                  x86_64   0.9.10-7.el8                          appstream               33 k
 libXtst                     x86_64   1.2.3-7.el8                           appstream               22 k
 libdatrie                   x86_64   0.2.9-7.el8                           appstream               33 k
 libepoxy                    x86_64   1.5.8-1.el8                           appstream              225 k
 libfontenc                  x86_64   1.1.3-8.el8                           appstream               37 k
 libgusb                     x86_64   0.3.0-1.el8                           baseos                  49 k
 libjpeg-turbo               x86_64   1.5.3-12.el8                          appstream              157 k
 libmodman                   x86_64   2.0.1-17.el8                          baseos                  36 k
 libpkgconf                  x86_64   1.4.2-1.el8                           baseos                  35 k
 libpng                      x86_64   2:1.6.34-5.el8                        baseos                 126 k
 libproxy                    x86_64   0.4.15-5.2.el8                        baseos                  75 k
 libsoup                     x86_64   2.62.3-5.el8                          baseos                 439 k
 libthai                     x86_64   0.1.27-2.el8                          appstream              203 k
 libtiff                     x86_64   4.0.9-29.el8_8                        ubi-8-appstream-rpms   189 k
 libwayland-client           x86_64   1.21.0-1.el8                          appstream               41 k
 libwayland-cursor           x86_64   1.21.0-1.el8                          appstream               26 k
 libwayland-egl              x86_64   1.21.0-1.el8                          appstream               20 k
 libxcb                      x86_64   1.13.1-1.el8                          appstream              229 k
 libxkbcommon                x86_64   0.9.1-1.el8                           appstream              116 k
 lksctp-tools                x86_64   1.0.18-3.el8                          baseos                 100 k
 lua                         x86_64   5.3.4-12.el8                          appstream              192 k
 nspr                        x86_64   4.35.0-1.el8_8                        ubi-8-appstream-rpms   143 k
 nss                         x86_64   3.90.0-3.el8_8                        ubi-8-appstream-rpms   750 k
 nss-softokn                 x86_64   3.90.0-3.el8_8                        ubi-8-appstream-rpms   1.2 M
 nss-softokn-freebl          x86_64   3.90.0-3.el8_8                        ubi-8-appstream-rpms   389 k
 nss-sysinit                 x86_64   3.90.0-3.el8_8                        ubi-8-appstream-rpms    75 k
 nss-util                    x86_64   3.90.0-3.el8_8                        ubi-8-appstream-rpms   139 k
 pango                       x86_64   1.42.4-8.el8                          appstream              297 k
 pixman                      x86_64   0.38.4-4.el8                          appstream              258 k
 pkgconf                     x86_64   1.4.2-1.el8                           baseos                  38 k
 pkgconf-m4                  noarch   1.4.2-1.el8                           baseos                  17 k
 pkgconf-pkg-config          x86_64   1.4.2-1.el8                           baseos                  15 k
 rest                        x86_64   0.8.1-2.el8                           appstream               70 k
 shared-mime-info            x86_64   1.9-3.el8                             baseos                 329 k
 ttmkfdir                    x86_64   3.0.9-54.el8                          appstream               62 k
 tzdata-java                 noarch   2023c-1.el8                           appstream              267 k
 xkeyboard-config            noarch   2.28-1.el8                            appstream              782 k
 xorg-x11-font-utils         x86_64   1:7.5-41.el8                          appstream              104 k
 xorg-x11-fonts-Type1        noarch   7.5-19.el8                            appstream              522 k
Installing weak dependencies:
 abattis-cantarell-fonts     noarch   0.0.25-6.el8                          appstream              156 k
 dconf                       x86_64   0.28.0-4.el8                          appstream              108 k
 gtk3                        x86_64   3.22.30-11.el8                        appstream              4.5 M
Enabling module streams:
 javapackages-runtime                 201801                                                            

Transaction Summary
=========================================================================================================
Install  86 Packages

Total download size: 79 M
Installed size: 288 M
Downloading Packages:
(1/86): abattis-cantarell-fonts-0.0.25-6.el8.noarch.rpm                  556 kB/s | 156 kB     00:00    
(2/86): adwaita-cursor-theme-3.28.0-3.el8.noarch.rpm                     1.5 MB/s | 647 kB     00:00    
(3/86): alsa-lib-1.2.9-1.el8.x86_64.rpm                                  3.0 MB/s | 515 kB     00:00    
(4/86): at-spi2-atk-2.26.2-1.el8.x86_64.rpm                              1.3 MB/s |  89 kB     00:00    
(5/86): at-spi2-core-2.28.0-1.el8.x86_64.rpm                             1.9 MB/s | 169 kB     00:00    
(6/86): atk-2.28.1-1.el8.x86_64.rpm                                      2.4 MB/s | 272 kB     00:00    
(7/86): adwaita-icon-theme-3.28.0-3.el8.noarch.rpm                        17 MB/s |  11 MB     00:00    
(8/86): cairo-gobject-1.15.12-6.el8.x86_64.rpm                           432 kB/s |  33 kB     00:00    
(9/86): cairo-1.15.12-6.el8.x86_64.rpm                                   4.7 MB/s | 719 kB     00:00    
(10/86): dconf-0.28.0-4.el8.x86_64.rpm                                   1.4 MB/s | 108 kB     00:00    
(11/86): colord-libs-1.4.2-1.el8.x86_64.rpm                              2.5 MB/s | 236 kB     00:00    
(12/86): copy-jdk-configs-4.0-2.el8.noarch.rpm                           257 kB/s |  31 kB     00:00    
(13/86): gdk-pixbuf2-modules-2.36.12-5.el8.x86_64.rpm                    1.7 MB/s | 109 kB     00:00    
(14/86): fribidi-1.0.4-9.el8.x86_64.rpm                                  1.1 MB/s |  89 kB     00:00    
(15/86): graphite2-1.3.10-10.el8.x86_64.rpm                              1.8 MB/s | 122 kB     00:00    
(16/86): gtk-update-icon-cache-3.22.30-11.el8.x86_64.rpm                 450 kB/s |  32 kB     00:00    
(17/86): harfbuzz-1.7.5-4.el8.x86_64.rpm                                 2.7 MB/s | 296 kB     00:00    
(18/86): hicolor-icon-theme-0.17-2.el8.noarch.rpm                        527 kB/s |  49 kB     00:00    
(19/86): gtk3-3.22.30-11.el8.x86_64.rpm                                   21 MB/s | 4.5 MB     00:00    
(20/86): jasper-libs-2.0.14-5.el8.x86_64.rpm                             1.9 MB/s | 167 kB     00:00    
(21/86): jbigkit-libs-2.1-14.el8.x86_64.rpm                              851 kB/s |  55 kB     00:00    
(22/86): lcms2-2.9-2.el8.x86_64.rpm                                      2.6 MB/s | 165 kB     00:00    
(23/86): libX11-1.6.8-7.el8.x86_64.rpm                                   6.0 MB/s | 612 kB     00:00    
(24/86): libXau-1.0.9-3.el8.x86_64.rpm                                   715 kB/s |  37 kB     00:00    
(25/86): libX11-common-1.6.8-7.el8.noarch.rpm                            1.4 MB/s | 211 kB     00:00    
(26/86): libXcomposite-0.4.4-14.el8.x86_64.rpm                           440 kB/s |  28 kB     00:00    
(27/86): libXcursor-1.1.15-3.el8.x86_64.rpm                              558 kB/s |  36 kB     00:00    
(28/86): libXdamage-1.1.4-14.el8.x86_64.rpm                              577 kB/s |  27 kB     00:00    
(29/86): libXext-1.3.4-1.el8.x86_64.rpm                                  954 kB/s |  45 kB     00:00    
(30/86): libXfixes-5.0.3-7.el8.x86_64.rpm                                477 kB/s |  25 kB     00:00    
(31/86): libXft-2.3.3-1.el8.x86_64.rpm                                   1.1 MB/s |  67 kB     00:00    
(32/86): libXi-1.7.10-1.el8.x86_64.rpm                                   856 kB/s |  49 kB     00:00    
(33/86): libXinerama-1.1.4-1.el8.x86_64.rpm                              297 kB/s |  16 kB     00:00    
(34/86): libXrandr-1.5.2-1.el8.x86_64.rpm                                681 kB/s |  34 kB     00:00    
(35/86): libXrender-0.9.10-7.el8.x86_64.rpm                              594 kB/s |  33 kB     00:00    
(36/86): libXtst-1.2.3-7.el8.x86_64.rpm                                  357 kB/s |  22 kB     00:00    
(37/86): libdatrie-0.2.9-7.el8.x86_64.rpm                                467 kB/s |  33 kB     00:00    
(38/86): libepoxy-1.5.8-1.el8.x86_64.rpm                                 3.5 MB/s | 225 kB     00:00    
(39/86): libfontenc-1.1.3-8.el8.x86_64.rpm                               570 kB/s |  37 kB     00:00    
(40/86): libjpeg-turbo-1.5.3-12.el8.x86_64.rpm                           2.5 MB/s | 157 kB     00:00    
(41/86): libthai-0.1.27-2.el8.x86_64.rpm                                 2.8 MB/s | 203 kB     00:00    
(42/86): libwayland-client-1.21.0-1.el8.x86_64.rpm                       646 kB/s |  41 kB     00:00    
(43/86): libwayland-cursor-1.21.0-1.el8.x86_64.rpm                       514 kB/s |  26 kB     00:00    
(44/86): libwayland-egl-1.21.0-1.el8.x86_64.rpm                          407 kB/s |  20 kB     00:00    
(45/86): libxkbcommon-0.9.1-1.el8.x86_64.rpm                             2.2 MB/s | 116 kB     00:00    
(46/86): libxcb-1.13.1-1.el8.x86_64.rpm                                  3.2 MB/s | 229 kB     00:00    
(47/86): lua-5.3.4-12.el8.x86_64.rpm                                     2.8 MB/s | 192 kB     00:00    
(48/86): pango-1.42.4-8.el8.x86_64.rpm                                   4.2 MB/s | 297 kB     00:00    
(49/86): pixman-0.38.4-4.el8.x86_64.rpm                                  2.8 MB/s | 258 kB     00:00    
(50/86): rest-0.8.1-2.el8.x86_64.rpm                                     982 kB/s |  70 kB     00:00    
(51/86): ttmkfdir-3.0.9-54.el8.x86_64.rpm                                932 kB/s |  62 kB     00:00    
(52/86): tzdata-java-2023c-1.el8.noarch.rpm                              4.0 MB/s | 267 kB     00:00    
(53/86): xorg-x11-font-utils-7.5-41.el8.x86_64.rpm                       1.4 MB/s | 104 kB     00:00    
(54/86): xkeyboard-config-2.28-1.el8.noarch.rpm                          7.2 MB/s | 782 kB     00:00    
(55/86): xorg-x11-fonts-Type1-7.5-19.el8.noarch.rpm                      5.9 MB/s | 522 kB     00:00    
(56/86): avahi-libs-0.7-21.el8.x86_64.rpm                                930 kB/s |  62 kB     00:00    
(57/86): cups-libs-2.2.6-54.el8.x86_64.rpm                               4.5 MB/s | 436 kB     00:00    
(58/86): dejavu-fonts-common-2.35-7.el8.noarch.rpm                       1.1 MB/s |  74 kB     00:00    
(59/86): dejavu-sans-mono-fonts-2.35-7.el8.noarch.rpm                    7.7 MB/s | 447 kB     00:00    
(60/86): fontpackages-filesystem-1.44-22.el8.noarch.rpm                  621 kB/s |  16 kB     00:00    
(61/86): fontconfig-2.13.1-4.el8.x86_64.rpm                              7.3 MB/s | 274 kB     00:00    
(62/86): freetype-2.9.1-9.el8.x86_64.rpm                                  16 MB/s | 394 kB     00:00    
(63/86): glib-networking-2.56.1-1.1.el8.x86_64.rpm                       7.0 MB/s | 155 kB     00:00    
(64/86): gsettings-desktop-schemas-3.32.0-6.el8.x86_64.rpm                14 MB/s | 633 kB     00:00    
(65/86): libgusb-0.3.0-1.el8.x86_64.rpm                                  1.5 MB/s |  49 kB     00:00    
(66/86): gdk-pixbuf2-2.36.12-5.el8.x86_64.rpm                            5.9 MB/s | 467 kB     00:00    
(67/86): libpkgconf-1.4.2-1.el8.x86_64.rpm                               2.0 MB/s |  35 kB     00:00    
(68/86): libmodman-2.0.1-17.el8.x86_64.rpm                               1.5 MB/s |  36 kB     00:00    
(69/86): libpng-1.6.34-5.el8.x86_64.rpm                                  5.8 MB/s | 126 kB     00:00    
(70/86): libproxy-0.4.15-5.2.el8.x86_64.rpm                              2.8 MB/s |  75 kB     00:00    
(71/86): libsoup-2.62.3-5.el8.x86_64.rpm                                  16 MB/s | 439 kB     00:00    
(72/86): pkgconf-1.4.2-1.el8.x86_64.rpm                                  2.1 MB/s |  38 kB     00:00    
(73/86): lksctp-tools-1.0.18-3.el8.x86_64.rpm                            3.5 MB/s | 100 kB     00:00    
(74/86): pkgconf-m4-1.4.2-1.el8.noarch.rpm                               907 kB/s |  17 kB     00:00    
(75/86): pkgconf-pkg-config-1.4.2-1.el8.x86_64.rpm                       871 kB/s |  15 kB     00:00    
(76/86): shared-mime-info-1.9-3.el8.x86_64.rpm                            16 MB/s | 329 kB     00:00    
(77/86): javapackages-filesystem-5.3.0-1.module+el8+2447+6f56d9a6.noarch 451 kB/s |  30 kB     00:00    
(78/86): java-17-openjdk-17.0.9.0.9-2.el8.x86_64.rpm                     4.3 MB/s | 458 kB     00:00    
(79/86): libtiff-4.0.9-29.el8_8.x86_64.rpm                               2.6 MB/s | 189 kB     00:00    
(80/86): nspr-4.35.0-1.el8_8.x86_64.rpm                                  5.4 MB/s | 143 kB     00:00    
(81/86): nss-3.90.0-3.el8_8.x86_64.rpm                                    15 MB/s | 750 kB     00:00    
(82/86): nss-softokn-3.90.0-3.el8_8.x86_64.rpm                            19 MB/s | 1.2 MB     00:00    
(83/86): nss-softokn-freebl-3.90.0-3.el8_8.x86_64.rpm                     14 MB/s | 389 kB     00:00    
(84/86): nss-sysinit-3.90.0-3.el8_8.x86_64.rpm                           4.1 MB/s |  75 kB     00:00    
(85/86): nss-util-3.90.0-3.el8_8.x86_64.rpm                              7.4 MB/s | 139 kB     00:00    
(86/86): java-17-openjdk-headless-17.0.9.0.9-2.el8.x86_64.rpm             64 MB/s |  46 MB     00:00    
---------------------------------------------------------------------------------------------------------
Total                                                                     27 MB/s |  79 MB     00:02     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Running scriptlet: copy-jdk-configs-4.0-2.el8.noarch                                               1/1 
  Running scriptlet: java-17-openjdk-headless-1:17.0.9.0.9-2.el8.x86_64                              1/1 
  Preparing        :                                                                                 1/1 
  Installing       : nspr-4.35.0-1.el8_8.x86_64                                                     1/86 
  Running scriptlet: nspr-4.35.0-1.el8_8.x86_64                                                     1/86 
  Installing       : libpng-2:1.6.34-5.el8.x86_64                                                   2/86 
  Installing       : freetype-2.9.1-9.el8.x86_64                                                    3/86 
  Installing       : nss-util-3.90.0-3.el8_8.x86_64                                                 4/86 
  Installing       : fontpackages-filesystem-1.44-22.el8.noarch                                     5/86 
  Installing       : libjpeg-turbo-1.5.3-12.el8.x86_64                                              6/86 
  Installing       : abattis-cantarell-fonts-0.0.25-6.el8.noarch                                    7/86 
  Installing       : fontconfig-2.13.1-4.el8.x86_64                                                 8/86 
  Running scriptlet: fontconfig-2.13.1-4.el8.x86_64                                                 8/86 
  Installing       : pixman-0.38.4-4.el8.x86_64                                                     9/86 
  Installing       : libwayland-client-1.21.0-1.el8.x86_64                                         10/86 
  Installing       : atk-2.28.1-1.el8.x86_64                                                       11/86 
  Installing       : libwayland-cursor-1.21.0-1.el8.x86_64                                         12/86 
  Installing       : jasper-libs-2.0.14-5.el8.x86_64                                               13/86 
  Installing       : dejavu-fonts-common-2.35-7.el8.noarch                                         14/86 
  Installing       : dejavu-sans-mono-fonts-2.35-7.el8.noarch                                      15/86 
  Installing       : gsettings-desktop-schemas-3.32.0-6.el8.x86_64                                 16/86 
  Installing       : nss-softokn-freebl-3.90.0-3.el8_8.x86_64                                      17/86 
  Installing       : nss-softokn-3.90.0-3.el8_8.x86_64                                             18/86 
  Installing       : nss-3.90.0-3.el8_8.x86_64                                                     19/86 
  Installing       : nss-sysinit-3.90.0-3.el8_8.x86_64                                             20/86 
  Installing       : ttmkfdir-3.0.9-54.el8.x86_64                                                  21/86 
  Installing       : javapackages-filesystem-5.3.0-1.module+el8+2447+6f56d9a6.noarch               22/86 
  Installing       : shared-mime-info-1.9-3.el8.x86_64                                             23/86 
  Running scriptlet: shared-mime-info-1.9-3.el8.x86_64                                             23/86 
  Installing       : gdk-pixbuf2-2.36.12-5.el8.x86_64                                              24/86 
  Running scriptlet: gdk-pixbuf2-2.36.12-5.el8.x86_64                                              24/86 
  Installing       : gtk-update-icon-cache-3.22.30-11.el8.x86_64                                   25/86 
  Installing       : pkgconf-m4-1.4.2-1.el8.noarch                                                 26/86 
  Installing       : lksctp-tools-1.0.18-3.el8.x86_64                                              27/86 
  Running scriptlet: lksctp-tools-1.0.18-3.el8.x86_64                                              27/86 
  Installing       : libpkgconf-1.4.2-1.el8.x86_64                                                 28/86 
  Installing       : pkgconf-1.4.2-1.el8.x86_64                                                    29/86 
  Installing       : pkgconf-pkg-config-1.4.2-1.el8.x86_64                                         30/86 
  Installing       : libmodman-2.0.1-17.el8.x86_64                                                 31/86 
  Running scriptlet: libmodman-2.0.1-17.el8.x86_64                                                 31/86 
  Installing       : libproxy-0.4.15-5.2.el8.x86_64                                                32/86 
  Running scriptlet: libproxy-0.4.15-5.2.el8.x86_64                                                32/86 
  Installing       : glib-networking-2.56.1-1.1.el8.x86_64                                         33/86 
  Installing       : libsoup-2.62.3-5.el8.x86_64                                                   34/86 
  Installing       : rest-0.8.1-2.el8.x86_64                                                       35/86 
  Running scriptlet: rest-0.8.1-2.el8.x86_64                                                       35/86 
  Installing       : libgusb-0.3.0-1.el8.x86_64                                                    36/86 
  Installing       : avahi-libs-0.7-21.el8.x86_64                                                  37/86 
  Installing       : cups-libs-1:2.2.6-54.el8.x86_64                                               38/86 
  Installing       : xkeyboard-config-2.28-1.el8.noarch                                            39/86 
  Installing       : libxkbcommon-0.9.1-1.el8.x86_64                                               40/86 
  Installing       : tzdata-java-2023c-1.el8.noarch                                                41/86 
  Installing       : lua-5.3.4-12.el8.x86_64                                                       42/86 
  Installing       : copy-jdk-configs-4.0-2.el8.noarch                                             43/86 
  Installing       : libwayland-egl-1.21.0-1.el8.x86_64                                            44/86 
  Installing       : libfontenc-1.1.3-8.el8.x86_64                                                 45/86 
  Installing       : xorg-x11-font-utils-1:7.5-41.el8.x86_64                                       46/86 
  Installing       : xorg-x11-fonts-Type1-7.5-19.el8.noarch                                        47/86 
  Running scriptlet: xorg-x11-fonts-Type1-7.5-19.el8.noarch                                        47/86 
  Installing       : libepoxy-1.5.8-1.el8.x86_64                                                   48/86 
  Installing       : libdatrie-0.2.9-7.el8.x86_64                                                  49/86 
  Running scriptlet: libdatrie-0.2.9-7.el8.x86_64                                                  49/86 
  Installing       : libthai-0.1.27-2.el8.x86_64                                                   50/86 
  Running scriptlet: libthai-0.1.27-2.el8.x86_64                                                   50/86 
  Installing       : libXau-1.0.9-3.el8.x86_64                                                     51/86 
  Installing       : libxcb-1.13.1-1.el8.x86_64                                                    52/86 
  Installing       : libX11-common-1.6.8-7.el8.noarch                                              53/86 
  Installing       : libX11-1.6.8-7.el8.x86_64                                                     54/86 
  Installing       : libXext-1.3.4-1.el8.x86_64                                                    55/86 
  Installing       : libXrender-0.9.10-7.el8.x86_64                                                56/86 
  Installing       : cairo-1.15.12-6.el8.x86_64                                                    57/86 
  Installing       : libXi-1.7.10-1.el8.x86_64                                                     58/86 
  Installing       : libXfixes-5.0.3-7.el8.x86_64                                                  59/86 
  Installing       : libXtst-1.2.3-7.el8.x86_64                                                    60/86 
  Installing       : libXcomposite-0.4.4-14.el8.x86_64                                             61/86 
  Installing       : at-spi2-core-2.28.0-1.el8.x86_64                                              62/86 
  Running scriptlet: at-spi2-core-2.28.0-1.el8.x86_64                                              62/86 
  Installing       : at-spi2-atk-2.26.2-1.el8.x86_64                                               63/86 
  Running scriptlet: at-spi2-atk-2.26.2-1.el8.x86_64                                               63/86 
  Installing       : libXcursor-1.1.15-3.el8.x86_64                                                64/86 
  Installing       : libXdamage-1.1.4-14.el8.x86_64                                                65/86 
  Installing       : cairo-gobject-1.15.12-6.el8.x86_64                                            66/86 
  Installing       : libXft-2.3.3-1.el8.x86_64                                                     67/86 
  Installing       : libXrandr-1.5.2-1.el8.x86_64                                                  68/86 
  Installing       : libXinerama-1.1.4-1.el8.x86_64                                                69/86 
  Installing       : lcms2-2.9-2.el8.x86_64                                                        70/86 
  Running scriptlet: lcms2-2.9-2.el8.x86_64                                                        70/86 
  Installing       : colord-libs-1.4.2-1.el8.x86_64                                                71/86 
  Installing       : jbigkit-libs-2.1-14.el8.x86_64                                                72/86 
  Running scriptlet: jbigkit-libs-2.1-14.el8.x86_64                                                72/86 
  Installing       : libtiff-4.0.9-29.el8_8.x86_64                                                 73/86 
  Installing       : gdk-pixbuf2-modules-2.36.12-5.el8.x86_64                                      74/86 
  Installing       : hicolor-icon-theme-0.17-2.el8.noarch                                          75/86 
  Installing       : graphite2-1.3.10-10.el8.x86_64                                                76/86 
  Installing       : harfbuzz-1.7.5-4.el8.x86_64                                                   77/86 
  Running scriptlet: harfbuzz-1.7.5-4.el8.x86_64                                                   77/86 
  Installing       : fribidi-1.0.4-9.el8.x86_64                                                    78/86 
  Installing       : pango-1.42.4-8.el8.x86_64                                                     79/86 
  Running scriptlet: pango-1.42.4-8.el8.x86_64                                                     79/86 
  Installing       : dconf-0.28.0-4.el8.x86_64                                                     80/86 
  Installing       : alsa-lib-1.2.9-1.el8.x86_64                                                   81/86 
  Running scriptlet: alsa-lib-1.2.9-1.el8.x86_64                                                   81/86 
  Installing       : java-17-openjdk-headless-1:17.0.9.0.9-2.el8.x86_64                            82/86 
  Running scriptlet: java-17-openjdk-headless-1:17.0.9.0.9-2.el8.x86_64                            82/86 
  Installing       : adwaita-cursor-theme-3.28.0-3.el8.noarch                                      83/86 
  Installing       : adwaita-icon-theme-3.28.0-3.el8.noarch                                        84/86 
  Installing       : gtk3-3.22.30-11.el8.x86_64                                                    85/86 
  Installing       : java-17-openjdk-1:17.0.9.0.9-2.el8.x86_64                                     86/86 
  Running scriptlet: java-17-openjdk-1:17.0.9.0.9-2.el8.x86_64                                     86/86 
  Running scriptlet: nss-3.90.0-3.el8_8.x86_64                                                     86/86 
  Running scriptlet: copy-jdk-configs-4.0-2.el8.noarch                                             86/86 
  Running scriptlet: dconf-0.28.0-4.el8.x86_64                                                     86/86 
  Running scriptlet: java-17-openjdk-headless-1:17.0.9.0.9-2.el8.x86_64                            86/86 
  Running scriptlet: java-17-openjdk-1:17.0.9.0.9-2.el8.x86_64                                     86/86 
  Running scriptlet: fontconfig-2.13.1-4.el8.x86_64                                                86/86 
  Running scriptlet: shared-mime-info-1.9-3.el8.x86_64                                             86/86 
  Running scriptlet: gdk-pixbuf2-2.36.12-5.el8.x86_64                                              86/86 
  Running scriptlet: hicolor-icon-theme-0.17-2.el8.noarch                                          86/86 
  Running scriptlet: adwaita-icon-theme-3.28.0-3.el8.noarch                                        86/86 
  Verifying        : abattis-cantarell-fonts-0.0.25-6.el8.noarch                                    1/86 
  Verifying        : adwaita-cursor-theme-3.28.0-3.el8.noarch                                       2/86 
  Verifying        : adwaita-icon-theme-3.28.0-3.el8.noarch                                         3/86 
  Verifying        : alsa-lib-1.2.9-1.el8.x86_64                                                    4/86 
  Verifying        : at-spi2-atk-2.26.2-1.el8.x86_64                                                5/86 
  Verifying        : at-spi2-core-2.28.0-1.el8.x86_64                                               6/86 
  Verifying        : atk-2.28.1-1.el8.x86_64                                                        7/86 
  Verifying        : cairo-1.15.12-6.el8.x86_64                                                     8/86 
  Verifying        : cairo-gobject-1.15.12-6.el8.x86_64                                             9/86 
  Verifying        : colord-libs-1.4.2-1.el8.x86_64                                                10/86 
  Verifying        : copy-jdk-configs-4.0-2.el8.noarch                                             11/86 
  Verifying        : dconf-0.28.0-4.el8.x86_64                                                     12/86 
  Verifying        : fribidi-1.0.4-9.el8.x86_64                                                    13/86 
  Verifying        : gdk-pixbuf2-modules-2.36.12-5.el8.x86_64                                      14/86 
  Verifying        : graphite2-1.3.10-10.el8.x86_64                                                15/86 
  Verifying        : gtk-update-icon-cache-3.22.30-11.el8.x86_64                                   16/86 
  Verifying        : gtk3-3.22.30-11.el8.x86_64                                                    17/86 
  Verifying        : harfbuzz-1.7.5-4.el8.x86_64                                                   18/86 
  Verifying        : hicolor-icon-theme-0.17-2.el8.noarch                                          19/86 
  Verifying        : jasper-libs-2.0.14-5.el8.x86_64                                               20/86 
  Verifying        : jbigkit-libs-2.1-14.el8.x86_64                                                21/86 
  Verifying        : lcms2-2.9-2.el8.x86_64                                                        22/86 
  Verifying        : libX11-1.6.8-7.el8.x86_64                                                     23/86 
  Verifying        : libX11-common-1.6.8-7.el8.noarch                                              24/86 
  Verifying        : libXau-1.0.9-3.el8.x86_64                                                     25/86 
  Verifying        : libXcomposite-0.4.4-14.el8.x86_64                                             26/86 
  Verifying        : libXcursor-1.1.15-3.el8.x86_64                                                27/86 
  Verifying        : libXdamage-1.1.4-14.el8.x86_64                                                28/86 
  Verifying        : libXext-1.3.4-1.el8.x86_64                                                    29/86 
  Verifying        : libXfixes-5.0.3-7.el8.x86_64                                                  30/86 
  Verifying        : libXft-2.3.3-1.el8.x86_64                                                     31/86 
  Verifying        : libXi-1.7.10-1.el8.x86_64                                                     32/86 
  Verifying        : libXinerama-1.1.4-1.el8.x86_64                                                33/86 
  Verifying        : libXrandr-1.5.2-1.el8.x86_64                                                  34/86 
  Verifying        : libXrender-0.9.10-7.el8.x86_64                                                35/86 
  Verifying        : libXtst-1.2.3-7.el8.x86_64                                                    36/86 
  Verifying        : libdatrie-0.2.9-7.el8.x86_64                                                  37/86 
  Verifying        : libepoxy-1.5.8-1.el8.x86_64                                                   38/86 
  Verifying        : libfontenc-1.1.3-8.el8.x86_64                                                 39/86 
  Verifying        : libjpeg-turbo-1.5.3-12.el8.x86_64                                             40/86 
  Verifying        : libthai-0.1.27-2.el8.x86_64                                                   41/86 
  Verifying        : libwayland-client-1.21.0-1.el8.x86_64                                         42/86 
  Verifying        : libwayland-cursor-1.21.0-1.el8.x86_64                                         43/86 
  Verifying        : libwayland-egl-1.21.0-1.el8.x86_64                                            44/86 
  Verifying        : libxcb-1.13.1-1.el8.x86_64                                                    45/86 
  Verifying        : libxkbcommon-0.9.1-1.el8.x86_64                                               46/86 
  Verifying        : lua-5.3.4-12.el8.x86_64                                                       47/86 
  Verifying        : pango-1.42.4-8.el8.x86_64                                                     48/86 
  Verifying        : pixman-0.38.4-4.el8.x86_64                                                    49/86 
  Verifying        : rest-0.8.1-2.el8.x86_64                                                       50/86 
  Verifying        : ttmkfdir-3.0.9-54.el8.x86_64                                                  51/86 
  Verifying        : tzdata-java-2023c-1.el8.noarch                                                52/86 
  Verifying        : xkeyboard-config-2.28-1.el8.noarch                                            53/86 
  Verifying        : xorg-x11-font-utils-1:7.5-41.el8.x86_64                                       54/86 
  Verifying        : xorg-x11-fonts-Type1-7.5-19.el8.noarch                                        55/86 
  Verifying        : avahi-libs-0.7-21.el8.x86_64                                                  56/86 
  Verifying        : cups-libs-1:2.2.6-54.el8.x86_64                                               57/86 
  Verifying        : dejavu-fonts-common-2.35-7.el8.noarch                                         58/86 
  Verifying        : dejavu-sans-mono-fonts-2.35-7.el8.noarch                                      59/86 
  Verifying        : fontconfig-2.13.1-4.el8.x86_64                                                60/86 
  Verifying        : fontpackages-filesystem-1.44-22.el8.noarch                                    61/86 
  Verifying        : freetype-2.9.1-9.el8.x86_64                                                   62/86 
  Verifying        : gdk-pixbuf2-2.36.12-5.el8.x86_64                                              63/86 
  Verifying        : glib-networking-2.56.1-1.1.el8.x86_64                                         64/86 
  Verifying        : gsettings-desktop-schemas-3.32.0-6.el8.x86_64                                 65/86 
  Verifying        : libgusb-0.3.0-1.el8.x86_64                                                    66/86 
  Verifying        : libmodman-2.0.1-17.el8.x86_64                                                 67/86 
  Verifying        : libpkgconf-1.4.2-1.el8.x86_64                                                 68/86 
  Verifying        : libpng-2:1.6.34-5.el8.x86_64                                                  69/86 
  Verifying        : libproxy-0.4.15-5.2.el8.x86_64                                                70/86 
  Verifying        : libsoup-2.62.3-5.el8.x86_64                                                   71/86 
  Verifying        : lksctp-tools-1.0.18-3.el8.x86_64                                              72/86 
  Verifying        : pkgconf-1.4.2-1.el8.x86_64                                                    73/86 
  Verifying        : pkgconf-m4-1.4.2-1.el8.noarch                                                 74/86 
  Verifying        : pkgconf-pkg-config-1.4.2-1.el8.x86_64                                         75/86 
  Verifying        : shared-mime-info-1.9-3.el8.x86_64                                             76/86 
  Verifying        : javapackages-filesystem-5.3.0-1.module+el8+2447+6f56d9a6.noarch               77/86 
  Verifying        : java-17-openjdk-1:17.0.9.0.9-2.el8.x86_64                                     78/86 
  Verifying        : java-17-openjdk-headless-1:17.0.9.0.9-2.el8.x86_64                            79/86 
  Verifying        : libtiff-4.0.9-29.el8_8.x86_64                                                 80/86 
  Verifying        : nspr-4.35.0-1.el8_8.x86_64                                                    81/86 
  Verifying        : nss-3.90.0-3.el8_8.x86_64                                                     82/86 
  Verifying        : nss-softokn-3.90.0-3.el8_8.x86_64                                             83/86 
  Verifying        : nss-softokn-freebl-3.90.0-3.el8_8.x86_64                                      84/86 
  Verifying        : nss-sysinit-3.90.0-3.el8_8.x86_64                                             85/86 
  Verifying        : nss-util-3.90.0-3.el8_8.x86_64                                                86/86 
Installed products updated.

Installed:
  abattis-cantarell-fonts-0.0.25-6.el8.noarch                                                            
  adwaita-cursor-theme-3.28.0-3.el8.noarch                                                               
  adwaita-icon-theme-3.28.0-3.el8.noarch                                                                 
  alsa-lib-1.2.9-1.el8.x86_64                                                                            
  at-spi2-atk-2.26.2-1.el8.x86_64                                                                        
  at-spi2-core-2.28.0-1.el8.x86_64                                                                       
  atk-2.28.1-1.el8.x86_64                                                                                
  avahi-libs-0.7-21.el8.x86_64                                                                           
  cairo-1.15.12-6.el8.x86_64                                                                             
  cairo-gobject-1.15.12-6.el8.x86_64                                                                     
  colord-libs-1.4.2-1.el8.x86_64                                                                         
  copy-jdk-configs-4.0-2.el8.noarch                                                                      
  cups-libs-1:2.2.6-54.el8.x86_64                                                                        
  dconf-0.28.0-4.el8.x86_64                                                                              
  dejavu-fonts-common-2.35-7.el8.noarch                                                                  
  dejavu-sans-mono-fonts-2.35-7.el8.noarch                                                               
  fontconfig-2.13.1-4.el8.x86_64                                                                         
  fontpackages-filesystem-1.44-22.el8.noarch                                                             
  freetype-2.9.1-9.el8.x86_64                                                                            
  fribidi-1.0.4-9.el8.x86_64                                                                             
  gdk-pixbuf2-2.36.12-5.el8.x86_64                                                                       
  gdk-pixbuf2-modules-2.36.12-5.el8.x86_64                                                               
  glib-networking-2.56.1-1.1.el8.x86_64                                                                  
  graphite2-1.3.10-10.el8.x86_64                                                                         
  gsettings-desktop-schemas-3.32.0-6.el8.x86_64                                                          
  gtk-update-icon-cache-3.22.30-11.el8.x86_64                                                            
  gtk3-3.22.30-11.el8.x86_64                                                                             
  harfbuzz-1.7.5-4.el8.x86_64                                                                            
  hicolor-icon-theme-0.17-2.el8.noarch                                                                   
  jasper-libs-2.0.14-5.el8.x86_64                                                                        
  java-17-openjdk-1:17.0.9.0.9-2.el8.x86_64                                                              
  java-17-openjdk-headless-1:17.0.9.0.9-2.el8.x86_64                                                     
  javapackages-filesystem-5.3.0-1.module+el8+2447+6f56d9a6.noarch                                        
  jbigkit-libs-2.1-14.el8.x86_64                                                                         
  lcms2-2.9-2.el8.x86_64                                                                                 
  libX11-1.6.8-7.el8.x86_64                                                                              
  libX11-common-1.6.8-7.el8.noarch                                                                       
  libXau-1.0.9-3.el8.x86_64                                                                              
  libXcomposite-0.4.4-14.el8.x86_64                                                                      
  libXcursor-1.1.15-3.el8.x86_64                                                                         
  libXdamage-1.1.4-14.el8.x86_64                                                                         
  libXext-1.3.4-1.el8.x86_64                                                                             
  libXfixes-5.0.3-7.el8.x86_64                                                                           
  libXft-2.3.3-1.el8.x86_64                                                                              
  libXi-1.7.10-1.el8.x86_64                                                                              
  libXinerama-1.1.4-1.el8.x86_64                                                                         
  libXrandr-1.5.2-1.el8.x86_64                                                                           
  libXrender-0.9.10-7.el8.x86_64                                                                         
  libXtst-1.2.3-7.el8.x86_64                                                                             
  libdatrie-0.2.9-7.el8.x86_64                                                                           
  libepoxy-1.5.8-1.el8.x86_64                                                                            
  libfontenc-1.1.3-8.el8.x86_64                                                                          
  libgusb-0.3.0-1.el8.x86_64                                                                             
  libjpeg-turbo-1.5.3-12.el8.x86_64                                                                      
  libmodman-2.0.1-17.el8.x86_64                                                                          
  libpkgconf-1.4.2-1.el8.x86_64                                                                          
  libpng-2:1.6.34-5.el8.x86_64                                                                           
  libproxy-0.4.15-5.2.el8.x86_64                                                                         
  libsoup-2.62.3-5.el8.x86_64                                                                            
  libthai-0.1.27-2.el8.x86_64                                                                            
  libtiff-4.0.9-29.el8_8.x86_64                                                                          
  libwayland-client-1.21.0-1.el8.x86_64                                                                  
  libwayland-cursor-1.21.0-1.el8.x86_64                                                                  
  libwayland-egl-1.21.0-1.el8.x86_64                                                                     
  libxcb-1.13.1-1.el8.x86_64                                                                             
  libxkbcommon-0.9.1-1.el8.x86_64                                                                        
  lksctp-tools-1.0.18-3.el8.x86_64                                                                       
  lua-5.3.4-12.el8.x86_64                                                                                
  nspr-4.35.0-1.el8_8.x86_64                                                                             
  nss-3.90.0-3.el8_8.x86_64                                                                              
  nss-softokn-3.90.0-3.el8_8.x86_64                                                                      
  nss-softokn-freebl-3.90.0-3.el8_8.x86_64                                                               
  nss-sysinit-3.90.0-3.el8_8.x86_64                                                                      
  nss-util-3.90.0-3.el8_8.x86_64                                                                         
  pango-1.42.4-8.el8.x86_64                                                                              
  pixman-0.38.4-4.el8.x86_64                                                                             
  pkgconf-1.4.2-1.el8.x86_64                                                                             
  pkgconf-m4-1.4.2-1.el8.noarch                                                                          
  pkgconf-pkg-config-1.4.2-1.el8.x86_64                                                                  
  rest-0.8.1-2.el8.x86_64                                                                                
  shared-mime-info-1.9-3.el8.x86_64                                                                      
  ttmkfdir-3.0.9-54.el8.x86_64                                                                           
  tzdata-java-2023c-1.el8.noarch                                                                         
  xkeyboard-config-2.28-1.el8.noarch                                                                     
  xorg-x11-font-utils-1:7.5-41.el8.x86_64                                                                
  xorg-x11-fonts-Type1-7.5-19.el8.noarch                                                                 

Complete!
[root@stapp03 banner]# 
```
-   Back to jenkins.
-  After install java  - > Launch agent up node 

![17](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/09a40ec8-19e3-4e46-af4f-b3a05f0e7119)


**5. Click on the  `Gitea`  button on the top bar to access the Gitea UI. Login using username  `sarah`  and password  `Sarah_pass123`.**

![19](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/2c343fba-4df8-41c5-9417-510cd6909d3d)


-   Click the repository named  `sarah/web-app`  in Gitea.  Inside Docker images. - `copy the Git link.`

![20](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/3a0ce591-70a5-45d3-b034-c7b14fed3ab7)




**6. Create a Jenkins pipeline job named `node-app`**

![18](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ec4c72a6-a970-4370-abca-c00730e7b049)

-   Click ok.
    
-   Create the pipeline script.

```
pipeline {
    agent {
        label 'app03'
    }
    
    stages {
        stage('Build') {
            steps {
                git branch: "master",
                    url: "http://git.stratos.xfusioncorp.com/sarah/web.git"
                
                sh 'docker build -t stregi01.stratos.xfusioncorp.com:5000/node-app:latest .'
                sh 'docker push stregi01.stratos.xfusioncorp.com:5000/node-app:latest'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d --name node-app -p 8080:8080 stregi01.stratos.xfusioncorp.com:5000/node-app:latest'
            }
        }

    }
}
```

![21](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/878b94ef-aa1a-4b2f-82d3-de4f0ad88e4e)

- Click save.

- Click Build Now

![22](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d273657a-3def-4f96-abb1-2df6ee8bc771)

### Consule output


```
# Console Output

Started by user [admin](https://8080-port-f4839ce93b0749ef.labs.kodekloud.com/user/admin)
[Pipeline] Start of Pipeline [Pipeline] node Running on [app03](https://8080-port-f4839ce93b0749ef.labs.kodekloud.com/manage/computer/app03/) in /home/banner/workspace/node-app [Pipeline] { [Pipeline] stage [Pipeline] { (Build) [Pipeline] git The recommended git tool is: NONE
No credentials specified
Cloning the remote Git repository
Cloning repository [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git)
 > git init /home/banner/workspace/node-app # timeout=10
Fetching upstream changes from [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git)
 > git --version # timeout=10
 > git --version # 'git version 2.39.3'
 > git fetch --tags --force --progress -- [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url [http://git.stratos.xfusioncorp.com/sarah/web.git](http://git.stratos.xfusioncorp.com/sarah/web.git) # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
Checking out Revision c2da484933db4ae0d00a0c6fc5b14c01756ef697 (refs/remotes/origin/master)
Commit message: "Added Dockerfile"
First time build. Skipping changelog.
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git config core.sparsecheckout # timeout=10
 > git checkout -f c2da484933db4ae0d00a0c6fc5b14c01756ef697 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git checkout -b master c2da484933db4ae0d00a0c6fc5b14c01756ef697 # timeout=10 [Pipeline] sh + docker build -t stregi01.stratos.xfusioncorp.com:5000/node-app:latest .
Sending build context to Docker daemon  2.697MB

Step 1/9 : FROM node:10-alpine
10-alpine: Pulling from library/node
ddad3d7c1e96: Pulling fs layer
de915e575d22: Pulling fs layer
7150aa69525b: Pulling fs layer
d7aa47be044e: Pulling fs layer
d7aa47be044e: Waiting
7150aa69525b: Verifying Checksum
7150aa69525b: Download complete
ddad3d7c1e96: Verifying Checksum
ddad3d7c1e96: Download complete
d7aa47be044e: Verifying Checksum
d7aa47be044e: Download complete
de915e575d22: Verifying Checksum
de915e575d22: Download complete 

ddad3d7c1e96: Pull complete

de915e575d22: Pull complete

7150aa69525b: Pull complete

d7aa47be044e: Pull complete
Digest: sha256:dc98dac24efd4254f75976c40bce46944697a110d06ce7fa47e7268470cf2e28
Status: Downloaded newer image for node:10-alpine
 ---> aa67ba258e18
Step 2/9 : RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app

 ---> Running in 4a701eea2e9c

Removing intermediate container 4a701eea2e9c
 ---> ddfcb81bda54
Step 3/9 : WORKDIR /home/node/app

 ---> Running in d8915946ab7a

Removing intermediate container d8915946ab7a
 ---> 4f59533e3274
Step 4/9 : COPY package*.json ./

 ---> 20515c1b2423
Step 5/9 : USER node

 ---> Running in b324b7c60c75

Removing intermediate container b324b7c60c75
 ---> 432a20d35fcd
Step 6/9 : RUN npm install
 ---> Running in c67239accfce

[91mnpm WARN nodejs-image-demo@1.0.0 No repository field.
[0m[91m
[0madded 50 packages from 37 contributors and audited 50 packages in 1.531s
found 2 high severity vulnerabilities
  run `npm audit fix` to fix them, or `npm audit` for details

Removing intermediate container c67239accfce
 ---> 45d41bd90296
Step 7/9 : COPY --chown=node:node . .

 ---> ae78ed133ddb
Step 8/9 : EXPOSE 8080

 ---> Running in f76f60f21635

Removing intermediate container f76f60f21635
 ---> a0783d604b62
Step 9/9 : CMD [ "node", "app.js" ]

 ---> Running in 18250fab1539

Removing intermediate container 18250fab1539
 ---> 6e2cc9266436

Successfully built 6e2cc9266436
Successfully tagged stregi01.stratos.xfusioncorp.com:5000/node-app:latest
[Pipeline] sh + docker push stregi01.stratos.xfusioncorp.com:5000/node-app:latest
The push refers to repository [stregi01.stratos.xfusioncorp.com:5000/node-app]
fd772ce2c323: Preparing
859350331e80: Preparing
221a9bf59372: Preparing
7b3578b8e0d4: Preparing
edff9ff691d5: Preparing
cbe4b9146f86: Preparing
a6524c5b12a6: Preparing
9a5d14f9f550: Preparing
cbe4b9146f86: Waiting
9a5d14f9f550: Waiting
a6524c5b12a6: Waiting 

7b3578b8e0d4: Pushed
221a9bf59372: Pushed
edff9ff691d5: Pushed
fd772ce2c323: Pushed

cbe4b9146f86: Pushed
9a5d14f9f550: Pushed

859350331e80: Pushed

a6524c5b12a6: Pushed
latest: digest: sha256:f2f1f6b0b5b10805444bf65c7ef2db8e1aaefc2d9c5b0b36a0fc95db434e8ebf size: 1995
[Pipeline] } [Pipeline] // stage [Pipeline] stage [Pipeline] { (Deploy) [Pipeline] sh 

+ docker run -d --name node-app -p 8080:8080 stregi01.stratos.xfusioncorp.com:5000/node-app:latest 

8e1b0cdad7e877e23bce1ff7e6f6b98f3a6589466530f15b060faea78dd912fa

[Pipeline] } [Pipeline] // stage [Pipeline] } [Pipeline] // node [Pipeline] End of Pipeline Finished: SUCCESS

```

![23](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/43e5649b-632b-48ad-8165-32acb1b83271)


![24](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/28241303-3f97-4076-9347-6bab9786d4f0)


**7. Validate the task**

### Back to session terminal 
  
 - Before Build.
```
thor@jump_host ~$ curl stapp03:8080
curl: (7) Failed to connect to stapp03 port 8080: Connection refused
thor@jump_host ~$ 
```
-  After Build
```
thor@jump_host ~$ curl stapp03:8080
<!DOCTYPE html>
<html lang="en">

<head>
    <title>About Sharks</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
    <link href="css/styles.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css?family=Merriweather:400,700" rel="stylesheet" type="text/css">
</head>

<body>
    <nav class="navbar navbar-dark bg-dark navbar-static-top navbar-expand-md">
        <div class="container">
            <button type="button" class="navbar-toggler collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false"> <span class="sr-only">Toggle navigation</span>
            </button> <a class="navbar-brand" href="#">Everything Sharks</a>
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav mr-auto">
                    <li class="active nav-item"><a href="/" class="nav-link">Home</a>
                    </li>
                    <li class="nav-item"><a href="/sharks" class="nav-link">Sharks</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    <div class="jumbotron">
        <div class="container">
            <h1>Want to Learn About Sharks?</h1>
            <p>Are you ready to learn about sharks?</p>
            <br>
            <p><a class="btn btn-primary btn-lg" href="/sharks" role="button">Get Shark Info</a>
            </p>
        </div>
    </div>
    <div class="container">
        <div class="row">
            <div class="col-lg-6">
                <h3>Not all sharks are alike</h3>
                <p>Though some are dangerous, sharks generally do not attack humans. Out of the 500 species known to researchers, only 30 have been known to attack humans.
                </p>
            </div>
            <div class="col-lg-6">
                <h3>Sharks are ancient</h3>
                <p>There is evidence to suggest that sharks lived up to 400 million years ago.
                </p>
            </div>
        </div>
    </div>
</body>

</html>thor@jump_host ~$ 
```

- Before Build.

```
[root@stapp03 banner]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
[root@stapp03 banner]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@stapp03 banner]# 
```

- After Build.
```
[root@stapp03 banner]# docker images
REPOSITORY                                       TAG                 IMAGE ID            CREATED              SIZE
stregi01.stratos.xfusioncorp.com:5000/node-app   latest              6e2cc9266436        About a minute ago   87.4MB
node                                             10-alpine           aa67ba258e18        2 years ago          82.7MB
[root@stapp03 banner]# docker ps
CONTAINER ID        IMAGE                                                   COMMAND                  CREATED              STATUS              PORTS                    NAMES
8e1b0cdad7e8        stregi01.stratos.xfusioncorp.com:5000/node-app:latest   "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp   node-app
[root@stapp03 banner]# 
```


![25](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/03f7ff46-fe51-4714-bbc3-e99296545e6e)


**8. Click on `Finish` & `Confirm` to complete the task successfully**

![26](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e5882602-8146-4728-a1de-48890bf87075)