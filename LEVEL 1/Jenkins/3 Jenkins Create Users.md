

## Questions:
The Nautilus team is planning to use Jenkins for some of their CI/CD pipelines. DevOps team has just installed a fresh Jenkins server and they are configuring it further to be available for use.

Click on the Jenkins button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

It has only a sample job for now. A new developer has joined the Nautilus application development team and they want this user to be added to the Jenkins server as per requirements mentioned below:

1. Create a jenkins user named `yousuf` with `LQfKeWWxWD` password, their full name should be `Yousuf` (it is case sensitive).

2. Using `Project-based Matrix Authorization Strategy` assign `overall read` permission to `yousuf` user.

3. Remember to remove all permissions for `Anonymous` users (if given) and make sure `admin` user has overall `Administer` permissions.

4. There is one existing job, make sure `yousuf` only has `read` permissions to that job (we are not worried about other permissions like Agent, SCM, etc.).

Note:

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on (Restart Jenkins when installation is complete and no jobs are running) on plugin installation/update page i.e `update centre`. Also, in case Jenkins UI gets stuck when Jenkins service restarts in the back end, please make sure to refresh the UI page.

2. Do not immediately click on `Finish` button if you have restarted the Jenkins service, please wait for Jenkins login page to come back before finishing your task.

3. For these kind of scenarios that required changes to be done from a web UI, please take screenshots of your work so that you can share the same with us for review purpose (in case your task is marked incomplete or failed). You may also consider using a screen recording software such as loom.com to record and share your work.


## Solution: 

**1. Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8081 and click on Display Port. You should be able to access the Jenkins login page.**

- Login using username theadmin and Adm!n321 password

**3. create user of jenkins**

- manage jenkins -> create users -> add new user,password and full name -> save it.

**3. add the pluings:**

- manage pluing -> add plugin ->Matrix Authorization Strategy -> installed and restart the plugin

**4. add configure of global security:**

- manage jenkins -> security -> choose the Matrix Authorization Strategy-> select the admin -administer -> user -read , existing job.
->logout jenkins

**5. login with new created user and check permission:**

- enter the user name and password->  jenkins dashboard and click the "hello world" project.
- click chnages options and see the build history the click the #1 options 

```
Console Output
Started by user ha:////4FQZHkbqVv2P0XgruB/jQu3Jj8mEP1dnl4cjZj3qkHoMAAAAmh+LCAAAAAAAAP9b85aBtbiIQTGjNKU4P08vOT+vOD8nVc83PyU1x6OyILUoJzMv2y+/JJUBAhiZGBgqihhk0NSjKDWzXb3RdlLBUSYGJk8GtpzUvPSSDB8G5tKinBIGIZ+sxLJE/ZzEvHT94JKizLx0a6BxUmjGOUNodHsLgAyuEgY+/dLi1CL9kozUxJTczDwAl3UqgMMAAAA=Admin User
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/Helloworld
[Helloworld] $ /bin/sh -xe /tmp/jenkins8620846834964872245.sh
+ echo Hello world
Hello world
```

**6. click the project `finish` and successfully.**

