


## Questions:

The Nautilus application development team has shared that they are planning to deploy one newly developed application on Nautilus infra in Stratos DC. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:


a. Install and configure PostgreSQL database on Nautilus database server.

b. Create a database user kodekloud_rin and set its password to GyQkFRVNr3.

c. Create a database kodekloud_db7 and grant full permissions to user kodekloud_rin on this database.

d. Make appropriate settings to allow all local clients (local socket connections) to connect to the kodekloud_db7 database through kodekloud_rin user using md5 method (Please do not try to encrypt password with md5sum).

e. At the end its good to test the db connection using these new credentials from root user or server's sudo user.


## Solution:  


**1. At first login  to the database server & Switch to the root user** 

```

thor@jump_host ~$ ssh peter@stdb01
peter@stdb01's password: Sp!dy

[peter@stdb01 ~]$ sudo su -
[sudo] password for peter: Sp!dy

```

**2. Install PostgreSQL** 

```
[root@stdb01 ~]# yum -y install postgresql-server postgresql-contrib
```

**3. Post installation Initiate DB setup**   
```
[root@stdb01 ~]#  postgresql-setup initdb
```

**4. Enable, start & check the status of the service** 
```
[root@stdb01 ~]# systemctl enable postgresql
Created symlink from /etc/systemd/system/multi-user.target.wants/postgresql.service to /usr/lib/systemd/system/postgresql.service.

[root@stdb01 ~]#

[root@stdb01 ~]# systemctl start postgresql

[root@stdb01 ~]#

[root@stdb01 ~]# systemctl status postgresql
```

**5.  Now login to the Postgres  & Create user, database, and grant permission**

``` 
CREATE USER <user_asper_your task> WITH PASSWORD '<password_asper_your task>';

CREATE DATABASE <DB_asper_your task>;

GRANT ALL PRIVILEGES ON DATABASE "<DB_asper_your task>" to <user_asper_your task>;  

[root@stdb01 ~]# sudo -u postgres psql

postgres=# CREATE USER kodekloud_rin WITH PASSWORD 'GyQkFRVNr3';

CREATE ROLE

postgres=# CREATE DATABASE kodekloud_db7;

CREATE DATABASE

postgres=# GRANT ALL PRIVILEGES ON DATABASE "kodekloud_db7" to kodekloud_rin;

GRANT

postgres=# \q

```

**6.Edit postgresql.conf  &  do the below configuration changes uncomment** 

```
[root@stdb01 ~]# vi /var/lib/pgsql/data/postgresql.conf

open another terminal search /listen
# - Connection Settings -
#listen_addresses = 'localhost' 
edit like this 
# - Connection Settings -
listen_addresses = 'localhost' 

exit control+c :wq

[root@stdb01 ~]# cat /var/lib/pgsql/data/postgresql.conf |grep localhost
```

**7. Edit another config file pg_hba.conf  &  configure the md5 at bottom of the config** 
```
[root@stdb01 ~]# vi /var/lib/pgsql/data/pg_hba.conf

open new terminal 
# "local" is for Unix domain socket connections only
local   all             all                                             md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                    md5

exit   :wq

[root@stdb01 ~]# cat /var/lib/pgsql/data/pg_hba.conf |grep md5
```

**8. Restart SQL service and check the status**
```
[root@stdb01 ~]# systemctl restart postgresql

[root@stdb01 ~]# systemctl status postgresql
```

**9.  Validate the task by below commands**
```
psql -U <user_asper_your _task> -d <DB_asper_your_task> -h 127.0.0.1 -W

psql -U <user_asper_your _task> -d <DB_asper_your_task> -h localhost -W

root@stdb01 ~]# psql -U kodekloud_rin -d kodekloud_db7 -h 127.0.0.1 -W
Password for user kodekloud_top:

psql (9.2.24)

Type "help" for help.

kodekloud_db7=> \q

[root@stdb01 ~]# psql -U kodekloud_rin -d kodekloud_db7 -h localhost -W
Password for user kodekloud_top:

psql (9.2.24)

Type "help" for help.

kodekloud_db7=>
```

**10.  Click on `Finish` & `Confirm` to complete the task successful**