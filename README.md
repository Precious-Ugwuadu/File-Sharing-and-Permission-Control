# Network File-Sharing And Permissions
<p align="center">
  
  ![image](https://github.com/user-attachments/assets/9b9c068e-c437-4bb1-b801-740a45b3b55f)

</p>

<h1>NFSP - Prerequisites and Installation</h1>
This lab demonstrates how to create shared folders on a domain controller (DC-1), apply different permission levels, and test access from a client Computer.<br />

<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>List of Prerequisites</h2>

- Windows Server VM (DC-1)
- Windows Client VM (Client-1)
- Domain admin and standard user accounts
- Functional Active Directory Domain
- File and Printer Sharing enabled on DC-1

<h2>Create some sample file shares with various permissions</h2>

<p>

![image](https://github.com/user-attachments/assets/a9fc5771-0045-4f64-8e01-551feec7e98f)

To begin, log in to DC-1 using your domain administrator account (mydomain.com\jane_admin).

![image](https://github.com/user-attachments/assets/02cbea81-17d4-4501-ab44-060a2c89a0d9)

Then, log in to Client-1 as a normal user (mydomain\bad.vew).

![image](https://github.com/user-attachments/assets/8fcabe78-baf0-40ec-8c81-89c4cf8f9f37)

On DC-1, create four folders on the C:\ drive named: "read-access", "write-access", "no-access", and "accounting".

HOW TO: 
Go to File Explorer > This PC > Windows (C:) > Right click > New > Folder ( Then go ahead and create all four folders)

Set the following permissions (share the folder)

 ![image](https://github.com/user-attachments/assets/95fbf337-b79a-4c5e-94b8-de3b11399b9d)

  a. Folder: “read-access”, Group: “Domain Users”, Permission: “Read”
  
  HOW TO: 
  Right click “read-access” > Properties > Sharing > Share > Type "Domain Users" > Add > Give "Read" > Share > Done. 

  ![image](https://github.com/user-attachments/assets/7a965112-2aee-44cc-9a8e-8c48b0613b1b)

  b. Folder: “write-access”,  Group: “Domain Users”, Permissions: “Read/Write”
  
  HOW TO: 
  Right click “read-access” > Properties > Sharing > Share > Type "Domain Users" > Add > Give "Read/Write" > Share > Done. 

  ![image](https://github.com/user-attachments/assets/1d2c12e1-b601-4d52-9ec8-127111948b16)
 
  c. Folder: “no-access”, Group: “Domain Admins”, “Permissions: “Read/Write”
  
  HOW TO: 
  Right click “read-access” > Properties > Sharing > Share > Type "Domain Admins" > Add > Give "Read/Write" > Share > Done. 
  
  d. (Skip accounting for now)

</p>

Now, try accessing the shared folders from Client-1 while logged in as a regular user (bad.vew), then attempt to open each of the shared folders. See which ones you can access? In which ones can you create or modify files? Do the results match the permissions you set?.

<p>

![image](https://github.com/user-attachments/assets/820cd25e-ef16-4bba-90b9-a7bcb247e688)
  
On Client-1, navigate to the shared folder (start, run, \\dc-1)

![image](https://github.com/user-attachments/assets/2b6b7724-2867-4460-925d-c2b706743e6b)

HOW TO:
Go to File Explorer > Type \\dc-1 > Enter > Double click "read-write" > New > Folder > (I got the above error message because I only have read access)

![image](https://github.com/user-attachments/assets/92191bc1-b32c-46cf-9664-06ec60ae2613)

I went to the "write-access" folder, and I can create a folder because I have "read-write" access.

![image](https://github.com/user-attachments/assets/466f0e17-73ea-4927-b2cb-3b234944ac23)

And with the "no-access" folder, I got the above error message. Because only "domain admins" have "read/write" access. 

NOTE:

As a normal user on Client-1, I tried to access the shared folders on DC-1. Based on the permissions:

  I could open "read-access" but not make changes (Read-only).

  I could open and create files in "write-access" (Read/Write).

  I couldn't access "no-access" because it's restricted to Domain Admins.

✅ This confirms that the folder access matches the permissions set on DC-1.

<h2>Create an “ACCOUNTANTS” Security Group, assign permissions, and test access</h2>

</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/a1bf900f-110e-43f4-839d-a4772658d221)
  
On DC-1, open Active Directory Users and Computers and create a new security group named “ACCOUNTANTS”.

HOW TO:

Right click mydomain.com > New > Organizational Unit > Name (_GROUPS) > OK > Refresh > Click on _GROUPS > Right Click > New > Group > Group name: "ACCOUNTANTS" > OK


![image](https://github.com/user-attachments/assets/79322788-4794-4a92-8cad-4f1cc7b30f5c)

On the “accounting” folder I created earlier, I will set the following permissions: Folder: “accounting”, Group: “ACCOUNTANTS”, Permissions: “Read/Write”

HOW TO:

Right click “accounting” > Properties > Sharing > Share > Type "ACCOUNTANTS" > Add > Give "Read/Write" > Share > Done. 

![image](https://github.com/user-attachments/assets/abbc8dcd-881f-4d6b-ae12-a288ec266096)

While logged into Client-1 as <bad.vew>, attempt to access the "accounting" shared folder — access should be denied.

HOW TO:
File Explorer > This PC > Type "\\dc-1" > Click on "accounting" > i saw the above error message because <bad.vew> isn't in the "ACCOUNTANTS" group, so they don’t have permission to access the folder.

Log out of Client-1 as  <bad.vew>

![image](https://github.com/user-attachments/assets/559f92b1-e0cd-4211-a830-80aaf54d6794)

On DC-1, make <bad.vew> a member of the “ACCOUNTANTS”  Security Group

HOW TO:

On DC-1, open Active Directory Users and Computers > mydomain.com > Double Click on _GROUPS > Double Click on ACCOUNTANTS > members > Add > bad.vew > Check Names > OK > Apply > OK

![image](https://github.com/user-attachments/assets/11899305-e00a-4925-8843-28f25ed7815d)

Log back into Client-1 as <bad.vew> 

![image](https://github.com/user-attachments/assets/ec78c0b0-128a-403c-b8b4-f7231b66b4a7)

Attempt to access the “accounting” share at \\DC-1\. Check if access is now granted.

HOW TO:

Go to File Explorer > \\dc-1 > Click on "accounting" and it should open > Right Click > New > New Text Document > and I can create a new document. 


🔍 Lab Summary (Very Brief)
In this lab, you created shared folders on DC-1 with different permission levels and tested access from Client-1 as a normal user. You also created a security group ("ACCOUNTANTS"), restricted folder access to that group, and verified access before and after adding the user to the group. This demonstrated how file sharing and group-based permissions work in a Windows domain.


</p>

<br /># File-Sharing-and-Permission-Control
