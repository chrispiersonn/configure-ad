<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- 1) Setup Domain Controller VM in Azure
- 2) Setup Client-1 VM in Azure
- 3) Install Active Directory
- 4) Create a Domain Admin user within the domain
- 5) Join Client-1 to your domain (mydomain.com)
- 6) Setup Remote Desktop for non-administrative users on Client-1
- 7) Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/734dc2db-34dc-4a1b-ab9c-331e2456c49a)
<p>
1) Create a Resource Group

Create a Virtual Network and Subnet

Create the Domain Controller VM (Windows Server 2022) named “DC-1” (Example)

Username: labuser (Example)

Password: Cyberlab123! (Example)
</p>
<br />

![image](https://github.com/user-attachments/assets/1cf17ef8-d735-4ef5-b01e-a6baf2d8c751)
![image](https://github.com/user-attachments/assets/4c1f98ec-03bf-456e-b428-c60c526e351b)
<p>
2) After VM is created, set Domain Controller’s NIC Private IP address to be static
</p>
<br />

![image](https://github.com/user-attachments/assets/e5cfa53b-55b5-46f7-be29-9bf84b4595d6)
<p>
3) Log into the VM and disable the Windows Firewall (for testing connectivity)
</p>
<br />

![image](https://github.com/user-attachments/assets/51280284-6a11-49ab-92df-80d5f906ab83)
<p>
4) Create the Client VM (Windows 10) named “Client-1” (Example)

Username: labuser (Example)

Password: Cyberlab123! (Example)

Attach it to the same region and Virtual Network as DC-1 (Example)
</p>
<br />

![image](https://github.com/user-attachments/assets/18bc1579-65c1-4a69-a794-50054a2f2d13)
![image](https://github.com/user-attachments/assets/641f59c1-9d7a-4a00-a5bc-cfd4702b530d)
<p>
5) After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address
</p>
<br />

![image](https://github.com/user-attachments/assets/0fe511c5-b488-4aee-ad0f-2c017abb62d7)
<p>
6) From the Azure Portal, restart Client-1
</p>
<br />

![image](https://github.com/user-attachments/assets/9da25c71-7065-4bdc-a0ef-3923e3d9e11c)
<p>
7) Login to Client-1

Attempt to ping DC-1’s private IP address

Ensure the ping succeeded
</p>
<br />

![image](https://github.com/user-attachments/assets/61a9332d-6d7e-46dd-8a6a-7d78de82aa4a)
<p>
8) From Client-1, open PowerShell and run ipconfig /all

The output for the DNS settings should show DC-1’s private IP Address
</p>
<br />

![image](https://github.com/user-attachments/assets/416689b5-c8bc-490b-aaeb-22f9cd53d7f6)
<p>
9) Login to DC-1 and install Active Directory Domain Services
</p>
<br />

![image](https://github.com/user-attachments/assets/2695f805-8f33-4908-9b2c-445015023fcd)
![image](https://github.com/user-attachments/assets/bbbff597-a215-44ad-8152-1299ba0ba809)
<p>
10) Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
</p>
<br />

![image](https://github.com/user-attachments/assets/7a923f98-e1ec-416e-9916-203ad76c1a8b)
<p>
11) Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

![image](https://github.com/user-attachments/assets/8c92709d-072d-4c8f-8c5b-9e7f5f165a7e)
<p>
12) In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

Create a new OU named “_ADMINS”
</p>
<br />

![image](https://github.com/user-attachments/assets/802e79f2-0a3b-41a1-b51c-60ea4aa801fc)
<p>
13) Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123! (Example)
</p>
<br />

![image](https://github.com/user-attachments/assets/a5c48e1f-ab6a-4628-bfdf-d7651ef6d217)
![image](https://github.com/user-attachments/assets/0d42eae6-9787-4d15-be20-9f2817aef0ac)
<p>
14) Add jane_admin to the “Domain Admins” Security Group
</p>
<br />

![image](https://github.com/user-attachments/assets/989239dd-33c6-45d6-b34f-b3182fd20066)
<p>
15) Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”

(Use jane_admin as your admin account from now on)
</p>
<br />

![image](https://github.com/user-attachments/assets/4bc9542b-3a90-4871-84cb-f71717929fb1)
![image](https://github.com/user-attachments/assets/5b1d304f-0aee-478c-8289-4d1ef56771d1)
<p>
16) Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)
</p>
<br />

![image](https://github.com/user-attachments/assets/b32417d2-b607-4313-a2c7-9cce10c85778)
<p>
17) Login to the Domain Controller and verify Client-1 shows up in ADUC
</p>
<br />

![image](https://github.com/user-attachments/assets/62b95ea8-485f-458b-8327-eccd20bbca62)
<p>
18) Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<br />

![image](https://github.com/user-attachments/assets/1fdba925-847f-4f08-8ddb-e0c0951b5f19)
![image](https://github.com/user-attachments/assets/b84d1d92-55df-412a-9470-1563b8aa560a)
<p>
19) Log into Client-1 as mydomain.com\jane_admin

Open system properties

Click “Remote Desktop”

Allow “domain users” access to remote desktop

You can now log into Client-1 as a normal, non-administrative user now (Normally you’d want to do this with Group Policy that allows you to change MANY systems at once)
</p>
<br />

![image](https://github.com/user-attachments/assets/848bb76f-cc20-4ce7-85c6-5371ff0d8441)
![image](https://github.com/user-attachments/assets/da9b851c-3914-4fcf-8bcc-935cb1731796)

20)Login to DC-1 as jane_admin

Open PowerShell_ise as an administrator

Create a new File and paste the contents of the [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into it

Run the script and observe the accounts being created
<br />

![image](https://github.com/user-attachments/assets/cd8aafb6-cc32-4254-b47c-cb718d329333)
<p>
21) When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES)

Attempt to log into Client-1 with one of the accounts (take note of the password in the script)
</p>
<br />
