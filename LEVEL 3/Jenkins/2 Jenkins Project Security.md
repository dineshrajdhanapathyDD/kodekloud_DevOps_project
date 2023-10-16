
## Questions

The xFusionCorp Industries has recruited some new developers. There are already some existing jobs on Jenkins and two of these new developers need permissions to access those jobs. The development team has already shared those requirements with the DevOps team, so as per details mentioned below grant required permissions to the developers.  
  

  

Click on the  `Jenkins`  button on the top bar to access the Jenkins UI. Login using username  `admin`  and password  `Adm!n321`.  
  

1.  There is an existing Jenkins job named  `Packages`, there are also two existing Jenkins users named  `sam`  with password  `sam@pass12345`  and  `rohan`  with password  `rohan@pass12345`.  
      
    
2.  Grant permissions to these users to access  `Packages`  job as per details mentioned below:  
      
    
    a.) Make sure to select  `Inherit permissions from parent ACL`  under  `inheritance strategy`  for granting permissions to these users.  
      
    
    b.) Grant mentioned permissions to  `sam`  user :  `build`,  `configure`  and  `read`.  
      
    
    c.) Grant mentioned permissions to  `rohan`  user :  `build`,  `cancel`,  `configure`,  `read`,  `update`  and  `tag`.  
      
    

`Note:`  
  

1.  Please do not modify/alter any other existing job configuration.  
      
    
2.  You might need to install some plugins and restart Jenkins service. So, we recommend clicking on  `Restart Jenkins when installation is complete and no jobs are running`  on plugin installation/update page i.e  `update centre`. Also Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case, please make sure to refresh the UI page.  
      
    
3.  For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

## Solution:

1.  Click on the + button in the top left corner and select option Select port to view on Host 1, enter port 8081 and click on Display Port. You should be able to access the Jenkins login page. Login using username the `admin` and `Adm!n321` password.

![1 login page](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/08d73476-df8b-4ff2-b221-45cea329f800)


![2 dashboard](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d02811ec-31c2-4b84-a254-dd574e3bfb48)


2.  Click Jenkins > Manage Jenkins > Manage Plugins and click Available tab.


![3 manage plugins](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/05a9c179-dd65-4e11-9264-40d0947cdddb)

Search for Matrix -> select Matrix Autherization Strategy plugin and click Download now and install after restart

![4 plugins choose](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/8b7a33be-8689-4ef4-a9b7-08c5df045ac7)

![6 download progress](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/cabe0749-bd6b-42b0-8c92-9d5d25f35770)
 
 - Restart jenkins

3. Click jenkins > manage jenkins > configure Global Security

- Authorization - choose project based authorization strategy option 

![5 authentication](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/67980d71-576e-4d7e-af42-58e472d9c077)

- Click save 

4.  Dashbaord > Packages  > Configuration

- Enable project based strategy
- check box Read sam

![7 sam project config](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/ea54e488-03cc-4a31-ad74-ca1b250c40d1)

- Click save
- Logout admin

5. Jenkins users named  `sam`  with password  `sam@pass12345` 

![8 sam login](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/03b37cda-b85f-4d86-a52c-4dd4710fc029)

- Access Denied

![9 sam denied](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d1ffc90a-b919-461f-9105-d3bf855a4cd1)

- logout sam

>Login Admin

- Create sam from admin Configuration

![10 sam create from admin](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/91ba06ed-74e7-40e7-844b-ad911213ea73)

- Admin configuration overall read checkbox click from sam.

![11 sam global config read](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/43f15d8f-f853-4b95-9d09-44fd0c51813c)

- click save.
- logout admin

 - Again login users named  `sam`  with password  `sam@pass12345` 

![8 sam login](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/03b37cda-b85f-4d86-a52c-4dd4710fc029)

![12 sam dasboard opn](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/be38e85c-8ff5-4d5b-a1c3-b01a421d2379)

- sam logout

> Login Admin

- Grant mentioned permissions to  `sam`  user :  `build`,  `configure`  and  `read`. 

![13 sam configuration admin](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/d5e1a704-f71e-4882-bf53-d562c3e2dffe)

- Click save 
- Admin Logout

> Login Sam

- Now ready for build and configure.
![14 sam build ready](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/bc9db770-2b3c-4a6d-8a1c-7d4d9f965841)
  
 - Click sam logout.


5.  Login Admin
 Dashbaord > Packages  > Configuration

- Enable project based strategy
- Create rohan 

![15 rohan create admin](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/c06d0a45-dab3-4cb3-8601-7c216cfe64c3)

- Grant mentioned permissions to  `rohan`  user :  `build`,  `cancel`,  `configure`,  `read`,  `update`  and  `tag`.  
![16 rohan project config](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/f691e74b-878c-4078-b700-53eac26ccb70)  

- Click save 
- Logout from Admin

6. Jenkins users named   `rohan`  with password  `rohan@pass12345`.  

![17 rohan login](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/06eb48e9-b26a-4c83-830e-5866d6cb6d1b)

- Access Denied

![18 rohan denied](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e11e7e55-e60f-48ca-8514-603d677ea375)

- logout rohan

> Login Admin
- Create rohan from admin Configuration
- Admin configuration overall read checkbox click from rohan.

![19 rohan global config](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/84cb2639-0311-48f0-980b-1f02951d79b8)

- click save.
- logout admin

Create Jenkins users named   `rohan`  with password  `rohan@pass12345`.  

![17 rohan login](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/06eb48e9-b26a-4c83-830e-5866d6cb6d1b)

![20 rohan dashboard](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/e7a7e78f-ac6b-4a97-b571-966da32a528a)


 - Dashboard > Packages  > Configuration

![21 rohan project config](https://github.com/dineshrajdhanapathyDD/kodekloud-Engineer_project/assets/52989362/636e2197-8e8e-4108-8d1b-230e40c78a9e)

7. Check the project to passed. 