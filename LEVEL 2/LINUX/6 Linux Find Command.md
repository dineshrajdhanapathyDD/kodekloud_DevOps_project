

## Questions:

During a routine security audit, the team identified an issue on the Nautilus App Server. Some malicious content was identified within the website code. After digging into the issue they found that there might be more infected files. Before doing a cleanup they would like to find all similar files and copy them to a safe location for further investigation. Accomplish the task as per the following requirements:



a. On App Server 2 at location `/var/www/html/ecommerce` find out all files (not directories) having .php extension.

b. Copy all those files along with their parent directory structure to location `/ecommerce` on same server.

c. Please make sure not to copy the entire `/var/www/html/`ecommerce directory content.


## Solution:

**1. At first login on App server ssh steve@stapp02**

```

thor@jump_host ~$ ssh steve@stapp02
steve@stapp02's password: Am3ric@
```

**2. Switch to root user : Sudo su -**

```
[steve@stapp02 ~]$ sudo su -
[sudo] password for steve: Am3ric@

[root@stapp02 ~]# ll /ecommerce
total 0
```

**3. Run the Below command to  find the file with  extension and  copy in a folder**

- Note:- Please  refer your task  and replace  extension  accordingly

```
[root@stapp02 ~]#  find /var/www/html/ecommerce -type f -name '*.php'  |wc -l
903 

 find /var/www/html/ecommerce -type f -name '*.php' -exec cp --parents {} /ecommerce \; 
```

**4. Validate the task by checking ll /ecommerce folder**

```

[root@stapp02 ~]# ll /ecommerce
total 4
```

**5.  Click on `Finish` & `Confirm` to complete the task successful**

