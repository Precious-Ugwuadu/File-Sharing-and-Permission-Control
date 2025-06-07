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

As a normal user on Client-1, you tried to access the shared folders on DC-1. Based on the permissions:

  I could open "read-access" but not make changes (Read-only).

  I could open and create files in "write-access" (Read/Write).

  I couldn't access "no-access" because it's restricted to Domain Admins.

✅ This confirms that the folder access matches the permissions set on DC-1.

<h2>Local DNS Cache Exercise</h2>

</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/532791cb-b44d-448c-90e2-074cd5208c8a)
  
Return to DC-1 and update the mainframe DNS A record to point to 8.8.8.8 instead of its original IP address.

![image](https://github.com/user-attachments/assets/25233fd8-9050-4150-b933-14a9b8c7e794)

On Client-1, ping "mainframe" again and notice that it still responds with the old IP address (10.0.0.4).
WHY?
It still pings the old IP address because Client-1 has cached the previous DNS record locally. The system uses this cached entry instead of querying the DNS server again. This is known as the local DNS cache, and it remains valid until it expires or is manually cleared. 

![image](https://github.com/user-attachments/assets/76ab5726-e04d-4d98-b039-cfb46ba967ed)

Check the local DNS cache by running ipconfig /displaydns > dns.txt > Enter > notepad dns.txt to view stored DNS entries on the system.

![image](https://github.com/user-attachments/assets/7f580c39-cb11-4755-afaf-4cc43918b705)

Run the command ipconfig /flushdns and ipconfig /displaydns to clear the saved DNS records on your computer, so it can fetch updated IP addresses from the DNS server. (Make sure to run this as an Admin). running ipconfig /flushdns cleared all previously stored DNS records. So when you run ipconfig /displaydns afterward, the cache appears empty since it was just wiped clean.

![image](https://github.com/user-attachments/assets/4e76f5b1-8834-4787-89a6-83d8636e5fe7)

Ping "mainframe" again and notice that the response now shows the updated IP address (8.8.8.8) from the new DNS record.

WHY?
Because the DNS cache was cleared, the system had to query the DNS server again. This time, it retrieved the updated A-record for “mainframe,” which reflects the new IP address you set.


<h2>CNAME Record Exercise</h2>
<p>
  
![image](https://github.com/user-attachments/assets/97cbf70e-15a7-4fe5-b959-6bc36b5a60b4)

</p>
<p>
"Return to DC-1 and create a CNAME record that maps the hostname 'search' to 'www.google.com'."
HOW TO:
On DC-1, Open DNS Manager > Expand dc-1 > Forward Lookup Zones > mydomain.com > Right click  > New Alias (CNAME) > (set up Alias name and FQDN) SEARCH > WWW.GOOGLE.COM > OK.  

![image](https://github.com/user-attachments/assets/b7d4d8b5-8079-4f1d-b11c-83d9c7e9332d)

Go back to Client-1 and attempt to ping “search”, observe the results of the CNAME record

![image](https://github.com/user-attachments/assets/126e8fb3-d9ea-40b1-887b-c4360a03cb20)

On Client-1, run nslookup search and check the output to see the CNAME record details.
why:
The nslookup command checks how DNS resolves the name search. If the CNAME record is set up properly, the output will show that search points to www.google.com.

![image](https://github.com/user-attachments/assets/ec3fdab6-c98a-481d-8ea8-643537157d0c)

I tried searching the name "https://search" and i got the above error message because the name does not match certificate of google.com

Lab Summary 
This lab covers:

A-Record Setup: You create a DNS A-record for mainframe so it resolves to an IP.
→ Shows how DNS maps names to IPs.

DNS Cache Behavior: Changing the IP in DNS doesn’t update immediately on the client due to caching.
→ Shows the importance of flushing DNS cache to get updated records.

CNAME Record: You create a CNAME so search points to www.google.com.
→ Demonstrates how aliases work in DNS.

Why It Matters
This lab teaches basic DNS management—how to add records, troubleshoot resolution issues, and understand caching.
</p>

<br /># File-Sharing-and-Permission-Control
