

## Questions:

The system admins team of xFusionCorp Industries needs to deploy a new application on `App Server 2` in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:



Install and configure nginx on `App Server 2`.

On App Server 2 there is a self signed SSL certificate and key present at location `/tmp/nautilus.crt` and `/tmp/nautilus.key`. Move them to some appropriate location and deploy the same in Nginx.

Create an `index.html` file with content `Welcome! `under Nginx document root.

For final testing try to access the App Server 2 link `(either hostname or IP)` from jump host using curl command. For example `curl -Ik https://<app-server-ip>/`.


## Solution: 

**1. At first login on App server as per the task &  Switch to  root user**

```

thor@jump_host ~$ ssh steve@stapp02
steve@stapp02's password: Am3ric@

[steve@stapp02 ~]$ sudo su -
[sudo] password for steve: Am3ric@
```

**2. Run Below command to install  epel repo & Nginx package**

```

[root@stapp02 ~]# yum install   epel-release -y

[root@stapp02 ~]# yum install   nginx -y

[root@stapp02 ~]# ll /tmp/
```

**3. Edit Nginx conf  & do the changes  as below**

Note  Server_name  IP should be the as per your app server mentioned in the task

```
[root@stapp02 ~]# vi /etc/nginx/nginx.conf

open another terminal to search - "/server_name"

edit the server_name 172.16.238.11;
uncomment the = Settings for a TLS enabled server.
ssl_certificate "/etc/pki/CA/certs/server.crt";
        
ssl_certificate_key "/etc/pki/CA/private/nautilus.key";
```

**4. Once the configuration is saved then copy the certificates and key**

```

[root@stapp02 ~]# cp /tmp/nautilus.crt /etc/pki/CA/certs/
[root@stapp02 ~]# cp /tmp/nautilus.key /etc/pki/CA/private/
[root@stapp02 ~]# ll /etc/pki/CA/certs/
total 8

[root@stapp02 ~]# cat /etc/nginx/nginx.conf |grep root
```

**5. Create an index.html with the word Welcome! on Nginx document root**

```

[root@stapp02 ~]# ll /usr/share/nginx/html/
total 16

[root@stapp02 ~]# rm /usr/share/nginx/html/index.html

[root@stapp02 ~]# vi /usr/share/nginx/html/index.html

[root@stapp02 ~]# cat /usr/share/nginx/html/index.html
Welcome!
```

**6. Start  & check the status of  the Nginx service**

```

[root@stapp02 ~]# systemctl start nginx

[root@stapp02 ~]# systemctl status nginx
```

**7. Click on `Finish` & `Confirm` to complete the task successful**