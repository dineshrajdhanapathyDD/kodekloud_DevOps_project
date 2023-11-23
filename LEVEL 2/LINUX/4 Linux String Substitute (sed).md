

## Questions:

There is some data on Nautilus App Server 2 in Stratos DC. Data needs to be altered in several of the files. On Nautilus App Server 2, alter the `/home/BSD.txt` file as per details given below:



a. Delete all lines containing software copyright and save results in `/home/BSD_DELETE.txt` file. (Please be aware of case sensitivity)

b. Replace all occurrence of word and to their and save results in `/home/BSD_REPLACE.txt` file.

Note: Let's say you are asked to replace word to with from. In that case, make sure not to alter any words containing this string; for example upto, contributor etc.



## solutions:

**1. Login on   App server as per the task**

```
thor@jump_host ~$ ssh steve@stapp02
pasword: Am3ric@


[steve@stapp02 ~]$ sudo su -
pasword: Am3ric@
```

**2. List the existing file and cat the word given in task which need to delete.**

```
[root@stapp02 ~]# ll /home/


[root@stapp02 ~]# cat /home/BSD.txt |grep software

(Note :- As per the task, BSD.txt file should be intact. But using "sed -i" will change the file in place.So if you want to use "sed -i" , make the copy of the BSD.txt to respective files and do the changes on newly created files.

Another option is which task required is just apply "sed" on original file and redirect the output to respective files. This won't affect the original place. As show below)
```

**3. Delete all lines containing word software and save results in `/home/BSD_DELETE.txt file`**

```
[root@stapp02 ~]# sed '/\<software\>/d' /home/BSD.txt > /home/BSD_DELETE.txt

[root@stapp02 ~]# cat /home/BSD_DELETE.txt |grep software
```

**4.  Cat the output for the word which going to replace**
```
[root@stapp02 ~]# cat /home/BSD.txt |grep and
```

**5.  Now replace  all occurrence of word and redirect to new file**
```
[root@stapp02 ~]# sed 's/\band\b/their/g' /home/BSD.txt > /home/BSD_REPLACE.txt
```

**6.  Validate the word replaced successfully**

```
[root@stapp02 ~]# cat /home/BSD_REPLACE.txt |grep and

[root@stapp02 ~]# cat /home/BSD_REPLACE.txt |grep their
```

**7. Click on `Finish` & `Confirm` to complete the task successful**