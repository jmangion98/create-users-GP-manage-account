  <p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Creating Users, Group Policy, and Managing Accounts in Azure </h1>
In this tutorial, we’ll configure Remote Desktop access for non-admin users, automate user creation with PowerShell, and manage Group Policy settings. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 
- Windows 10 

<h2>Project Walk-Through: </h2>

1. Configure Remote Desktop for Non-Admin Users:

- Log into Client-1 via Remote Desktop using the Domain Admin account (e.g., Jane_admin).
<br/>
  
![1stScreenshot-19c110f9-fcb8-4a53-a215-d63da58de20a](https://github.com/user-attachments/assets/85214ebc-8dc9-4d59-a177-add569534872)



</p>


- Go to Start > System > Remote Desktop and select "Users that can access this PC." Enter "Domain Users" and check the name. Now, all users can access Client-1 through Remote Desktop.
<br/>
  
![image](https://github.com/user-attachments/assets/b2d3c402-5890-4dbc-a6b0-0ce13640f534)

</p>

2. Automate User Creation with PowerShell:

- Log into the Domain Controller as an admin (e.g., Jane_admin).
<br/>
  

![2NDSCREENSHOT-380e3658-04fa-43c3-ab1d-69e031ef82b8](https://github.com/user-attachments/assets/14893c03-ea3b-414b-9872-a41bb8c0d918)


</p>


- Open PowerShell ISE in admin mode and save the script file.
<br />

![image](https://github.com/user-attachments/assets/36849256-6490-4303-8ab8-4d1b549f7b12)
![Screenshot 2024-10-04 141850](https://github.com/user-attachments/assets/d94e7ba6-24fc-4ab3-aa1d-a38b6fc825dd)


- Press "CTRL + S" to save the file.
<br/>

![Screenshot 2024-10-04 142109](https://github.com/user-attachments/assets/4e05f646-19e2-4980-b488-7978b63b876e)

<p/>
  

- Paste the user creation script in PowerShell ISE and run it. The script will create random users and store them in the "_EMPLOYEES" Organizational Unit (OU).
<br/> 

![image](https://github.com/user-attachments/assets/dc4ad2d4-8894-4599-9abc-e241bc18389d)

<p/>


- To verify, go to Active Directory Users and Computers > EMPLOYEES folder. Here, you’ll see the newly created users. - Pick one of the randomly generated users, then log into Client-1 using their username and the password "Password1
<br/>

![Screenshot 2024-10-04 143218](https://github.com/user-attachments/assets/56308577-7806-45dd-a572-539dcacc2574)

<p/>

3. Test User Login:

- Log out of Jane’s account on Client-1.
<br/>

![Screenshot 2024-10-04 143358](https://github.com/user-attachments/assets/bfd96e80-df73-40d5-97b1-10dc5a83c7ca)

<p/>


- Log back into Client-1 using the newly created user from the script. Make sure to specify the login context by adding "mydomain.com\" before entering the username
<br/>

![3RDSCREENSHOT-b300e9a8-e9b2-456c-92d8-8be2dbf16a56](https://github.com/user-attachments/assets/52b9a443-1efc-4093-b01b-7169a4a275f5)


<p/>


- Once logged in, navigate to C: > Users, we'll see that our new user has a folder, as do the other users. However, we cannot access these folders because we lack the necessary permissions.
<br/>

![Screenshot 2024-10-04 144108](https://github.com/user-attachments/assets/71cf0dd3-16b6-4f9f-8dc7-df828365f352)

<br/>


- Log out of this user account. Set up a group policy for account lockout and try to lock this user out.
<br/> 

![Screenshot 2024-10-04 144108](https://github.com/user-attachments/assets/5583c953-8c73-43ed-b7f4-49a92e4b2c25)

<br/>

4. Set Group Policy for Account Lockout:

- Return to the Domain Controller and open Group Policy Management Console (gpmc.msc).
<br/>

![image](https://github.com/user-attachments/assets/432b4858-dbe4-4d70-818b-699eeab2427a)
![Screenshot 2024-10-04 145946](https://github.com/user-attachments/assets/0ef8f684-c332-4437-b9fe-29b4f7cdf051)

<br/>


- Go to Domain > mydomain.com > Default Domain Policy.
<br/>

![Screenshot 2024-10-04 150413](https://github.com/user-attachments/assets/bfc8b25f-9539-49e4-ae6c-449c8aac7cae)

<br/>

- Navigate to Computer Configuration > Policies > Windows Settings > Security Settings > Account Lockout Policy.
<br/>

![Screenshot 2024-10-04 150620](https://github.com/user-attachments/assets/ee5581eb-8608-4122-98b2-4448f3b4abc5)

<br/>


- Select "Account Threshold" and set the account to lock after 5 invalid login attempts. You can also choose the duration of the lockout after the failed attempts. Once done, click "Apply."
<br/>

![Screenshot 2024-10-04 151109](https://github.com/user-attachments/assets/7ca43c7f-64e4-452f-a305-22a668d380a1)
![Screenshot 2024-10-04 151047](https://github.com/user-attachments/assets/fc87fc8f-f016-48bd-b1e2-4a573a2ca3bf)

<br/>

5. Verify Group Policy Update:
- To verify the changes, navigate to the Group Policy settings and review the lockout policy.
<br/>

![image](https://github.com/user-attachments/assets/cf33f8c5-67f8-42f8-975c-2345c3329181)

<br/>

- To update Group Policy immediately, log into Client-1 as an admin and run gpupdate /force in the command line.
<br/>

![4THSCREENSHOT-a059a3b3-5086-4e59-9cfa-95202c5f7fea](https://github.com/user-attachments/assets/1fbdfe9a-e59c-4b85-b1e2-297c3ff513ce)

![Screenshot 2024-10-04 152328](https://github.com/user-attachments/assets/58f43ed1-4be9-4dce-b9fa-7dce86d423ec)

<br/>

- Test the policy by logging into Client-1 with an incorrect password for the user more than 5 times. A lockout message should appear after the attempts.
<br/>

![5THSCREENSHOT-167cba41-9070-4a13-ae78-5671ce82e2f3](https://github.com/user-attachments/assets/473f666f-3cf4-4374-aa95-b0eb706ee334)


<br/>

6. Unlock a Locked User Account:

- Go to Active Directory Users and Computers > mydomain.com > _EMPLOYEES on the Domain Controller.
<br/>

![image](https://github.com/user-attachments/assets/f38a2b96-6441-4a06-81c7-1e84b3842935)

<br/>

- Double-click the locked user, go to the Account tab, and check the Unlock Account box. Optionally, reset the password. 
<br/>

![Screenshot 2024-10-04 153903](https://github.com/user-attachments/assets/5457df21-bcab-4ef8-b09a-1fe361abcf2f)

<br/>


- Now the user can log into Client-1 as normal
<br/>

![6THSCREENSHOT-ac1cb8a7-bd9d-4d3b-90ba-895d3befc8ed](https://github.com/user-attachments/assets/9ec926ca-c759-46d1-944d-26f007ddcc84)


<br/>

7. Disable and Enable a User Account:

- To disable a user, go to Active Directory Users and Computers > _EMPLOYEES, right-click the user, and select Disable.
<br/>

![image](https://github.com/user-attachments/assets/b33ac9be-3b6b-40e0-8e2f-7336215ff861)
![Screenshot 2024-10-04 154404](https://github.com/user-attachments/assets/33b36c14-7aed-4cb4-beec-75e08ad19829)


<br/>

- Attempting to log into Client-1 with a disabled account will produce an error.
<br/>

![7THSCREENSHOT-35ac1513-7664-4365-94ae-7b8d1b7d6975](https://github.com/user-attachments/assets/abb1b1c5-ad01-4c9c-bb46-04193d207d0e)


<br/>


- Navigate back to the DC and right-click the user thats disabled and select "Enable" 
<br/>

![image](https://github.com/user-attachments/assets/7fe48357-b6c1-479a-a360-0e0878a07541)
![Screenshot 2024-10-04 154711](https://github.com/user-attachments/assets/f14477e5-bce1-4917-a30f-b0761a5ab932)

<br/>

8. View User Logs in Event Viewer

- Search for Event Viewer, navigate to Windows Logs > Security.
- The disabled user is now re-enabled and has regained access to the account. Next, let’s attempt to view the user’s logs. To do this, search for "Event Viewer," then navigate to Windows Logs > Security. As you can see, we are unable to view the logs.
<br/>

![Screenshot 2024-10-04 155151](https://github.com/user-attachments/assets/2156dd78-8fc6-4e2f-b1d7-eb40831ea95f)

<br/>

<p align="center">
- We received the error in Event Viewer because we’re not logged in as an admin. Since we already know Jane's credentials, we can close the window and reopen it, this time as Administrator, and then enter the credentials.
<br/>

![Screenshot 2024-10-04 155418](https://github.com/user-attachments/assets/1731e2d1-17a9-4d0f-bfb7-2e0a2652e7bc)

<br/>

<p align=center">
- Now if we navigate back over to the Secuirty Log page we can view the logs: 
<br/>

![Screenshot 2024-10-04 155600](https://github.com/user-attachments/assets/7841dc9e-bda0-4758-91f8-5c78f6929dae)

<br/>

- Logs will display details like the IP address, failed login attempts, username, and date/time for each login attempt. If an error occurs, re-open Event Viewer as an administrator.
<br/>

![image](https://github.com/user-attachments/assets/c20d3ff0-afae-4fd1-9804-c021754c3f8c)














