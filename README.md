# Active-Directory-Basics

## Objective
The Active Directory Basics project aimed to demonstrate the different tools within Windows Active Directory that were utilized to manage permissions, authenticate users, and authorize users. This hands-on experience was designed to not only deepen understanding of Windows Active Directory but also understand the basic concepts of Remote Desktop Protocol (RDP).

### Content
- Deleting Extra OUs & Users
- Delegating Controls

### Skills Learned
- Enhanced knowledge of Active Directory concepts and tools.
- Utilizing Remote Desktop Protocol to verify Active Directory adjustments. 
- Development of critical thinking and problem-solving skills.

### Tools Used
- Windows Active Directory to perform changes of permission and users' access.
- Remote Desktop Protocol (RDP) to examine if changes made in Active Directory were correctly executed.

## Scenario
You'll assume the role of the new IT admin at THM Inc. We have been asked to review the current domain "THM.local" and do some additional configurations. You will have administrative credentials over a pre-configured Domain Controller (DC) to do the tasks. Login to the Administrator's account using the following credentials: 
- Username: Administrator
- Passowrd: Password321

### First Task: Deleting Extra OUs & Users
Your first task as the new domain administrator is to check the existing AD OUs and users, as some recent changes have happened to the business. You have been given the following organizational chart and are expected to make changes to the AD to match it:

![image](https://github.com/user-attachments/assets/a0cd2a8b-772a-4744-b543-85d9e238e630)
1. Search the Start menu and open the program **Active Directory Users and Controllers**. This will open a window which displays the hierarchy of users, computers, and groups in the domain. These objects are organized in Organizational Units (OUs) which are container objects that allow you to classify users and machines and used to define sets of users with similar policing requirements
2. Select the THM OU which will show five sub-OUs for the departments in the company. Need to remove the Research and Development OU as it's not included in the organizational chart.

   ![image](https://github.com/user-attachments/assets/914c656b-1c1c-45f4-9390-d5ea7f0c8ab9)
3. However, you can't simply delete it by right-clicking and selecting delete as an error message will pop up.

   ![image](https://github.com/user-attachments/assets/208eef7a-9fcd-4604-b57c-672aaaad53fe)
4. By default, OUs are protected against accidental deletion, so need to go about it another way to delete the OU. Under the View menu at the top select **Advanced Features** which will show you some additional containers and allow you to disable the accidental deletion protection.

   ![image](https://github.com/user-attachments/assets/eef0bd25-1f4d-4e70-873b-c89e0c45b076)
   ![image](https://github.com/user-attachments/assets/0e16feb5-790d-479f-8781-9940748d0df1)
5. Select the THM OU once more, then right-click the Research and Development OU and select Properties to open the Properties window. Under the Object section, uncheck the *Protect object from accidental deletion*. Then tey to delete the Research and Development OU once more and it should be successful. You will be prompted to confirm that you want to delete the OU, and as a result, any users, groups, or OUs under it will also be deleted.

   ![image](https://github.com/user-attachments/assets/837fdb4b-d9d3-4c72-b040-72429ccac9ee)
   ![image](https://github.com/user-attachments/assets/a9e22489-afab-4af0-86a9-2f677e5a0bb7)
6. Look through the rest of the departments to see if all the users under each OU are correct. In the Sales OU there seems to be more users than what is stated in the chart.

   ![image](https://github.com/user-attachments/assets/a3c6b1cc-46d4-4f3a-96c5-a60251ab4e96)
   - Sophie and Thomas are the only users that should be in the Sales OU
8. Need to remove Christine and Robert from the OU. Can simply remove them by right-clicking and selecting Delete to delete them, but a better way would be to disable their accounts. Deleting their accounts would completely remove their account along with the data associated with the account, including decryption needed for any encryption they may have added to data. Disabling the account prevents the user from being able to log into the network using their credentials, so it's safer to disable it until you're completely sure you don't need the account.

   ![image](https://github.com/user-attachments/assets/45574ca9-a321-463e-90f6-ae3f3e38099f)

### Delegating Controls
According to our organizational chart, Phillip is in charge of IT support, so we want to delegate the control of resetting passwords over the Sales, Marketing and Management OUs to him

1. Right-click one of the OUs that we want Phillip to have control over (such as the Sales OU) and select **Delegate Control**

   ![image](https://github.com/user-attachments/assets/607e6391-d2e3-41fb-bbc4-c62eed97d119)
2. The Delegation of Control window will open and click the **Add** button to add Phillip. Type Phillip and select **Check Names** to prevent any mistakes. Then select the OK button to close that window and select Next in the Delegation window

   ![image](https://github.com/user-attachments/assets/5f91b97b-d05a-4c44-aff8-c39ca66ebdaa)
3. Choose what task(s) to delegate to Phillip. In this case, we will delegate *Reset user passwords and force password change at next logon*. Click Next then Finish to exit out the window and repeat these steps with the rest of the OUs (Marketing and Management) 

   ![image](https://github.com/user-attachments/assets/1429a960-80dd-4b26-b70d-2d0996aceb4c)
4. Now let's use Phillip's account to try and reset Sophie's password to see if it worked. Log into Phillip's account using the credentials:
   - Username: phillip
   - Password: Claire2008

     ![image](https://github.com/user-attachments/assets/8aaf51a5-b429-4fc5-b558-98880e1f7025)
     - Can log out and log into Phillip's account or can open a terminal and input the command **xfreerdp 10.10.207.3** (10.10.207.3 is the target's IP address) to open RDP and log into Phillip's account
    
       ![image](https://github.com/user-attachments/assets/49e0fbba-abb9-45d1-bcd6-5865adf1eea6)
5. Phillip doesn't have the privilege to open **Active Directory Users and Computers**, so instead open Windows Powershell. Insert the command: **Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose** to change Sophie's password. Then choose a New Password for her account.

      ![image](https://github.com/user-attachments/assets/a247f451-93ed-4558-8f95-424b115883ac)
6. We can also force a password reset at the next logon using the command: **Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose**
7. Can check if the command went through by logging into Sophie's account using the username "sophie" and the new password we chose. 

 







