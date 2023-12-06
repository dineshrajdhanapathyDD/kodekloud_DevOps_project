

## Questions:
To stick with the security compliances, the Nautilus project team has decided to apply some restrictions on crontab access so that only allowed users can `create/update` the cron jobs. Limit crontab access to below specified users on (App Server 3).

Allow crontab access to `anita` user and deny the same to `rod` user.

## solutions:

**1. At first scp the file to stapp03**
```
thor@jump_host ~$ ssh banner@stapp03
banner@stapp03's password: BigGr33n

[banner@stapp03 ~]$ sudo su -
[sudo] password for banner: BigGr33n
```

**2. Find the crontabs**

```
[root@stapp03 ~]# rpm -qa |grep crontabs

[root@stapp03 ~]# systemctl status crond
● crond.service - Command Scheduler
   Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2023-07-26 18:22:03 UTC; 5min ago
 Main PID: 450 (crond)

[banner@stapp03 ~]$ cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

[root@stapp03 ~]# ls -l /var/spool/cron/
total 0

[root@stapp03 ~]# crontab -l
no crontab for root

User deny functions:
[root@stapp03 ~]# vi /etc/cron.deny
new terminal open

jerome

[root@stapp03 ~]# su - jerome

[jerome@stapp03 ~]$ crontab -e
You (jerome) are not allowed to use this program (crontab)
See crontab(1) for more information

[root@stapp03 ~]# ls -l /etc/cron.deny
-rw-r--r-- 1 root root 7 Jul 26 18:44 /etc/cron.deny


user allow functions:
[root@stapp03 ~]# vi /etc/cron.allow

[root@stapp03 ~]# su - siva
[siva@stapp03 ~]$ crontab -e

(no crontab for siva - using an empty one
crontab: installing new crontab
"/tmp/crontab.BdTQqK":1: bad minute
errors in crontab file, can't install.
Do you want to retry the same edit? y
[siva@stapp03 ~]$ corntab -l
-bash: corntab: command not found)


[siva@stapp03 ~]$ ls -l /etc/cron.allow
-rw-r--r-- 1 root root 5 Jul 26 18:50 /etc/cron.allow

[root@stapp03 ~]# ls -l /var/spool/cron/
total 0
-rw------- 1 root root 0 Jul 26 18:37 root
```