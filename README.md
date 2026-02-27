<p align="center">
  <img src="https://github.com/user-attachments/assets/7b65e021-0e08-42ae-8da2-f57cd11aceb7" width="300" height="168" alt="image" />
</p>

# Active Directory Enterprise Administration End-to-End Lab (Azure Virtual Environment)

This project demonstrates the complete deployment and administration of an enterprise-grade Active Directory environment hosted in Microsoft Azure. The lab walks through building a Windows Server domain controller, joining a Windows 10 client to the domain, configuring DNS and networking, and implementing real-world identity and access management operations. It includes creating organizational units, provisioning users, managing security groups, enforcing NTFS and share permissions, applying password and account lockout policies, configuring logon hour restrictions, and performing full user lifecycle operations—from creation to deprovisioning. This end-to-end lab replicates a true corporate IT environment and reinforces essential skills used in IT Support, Systems Administration, and Identity & Access Management roles.

---

## Environments and Technologies Used

- Microsoft Azure
- Windows Server 2022
- Windows 10 Pro Client
- Active Directory DOmain Services (AD DS)
- Networking & Protocols
- Tools & Utilities

---

## Table of Contents

- [1) Create Virtual Machines](#1-create-virtual-machines)
- [2) Make the Domain Controllers IP Static](#2-make-the-domain-controllers-ip-address-static)
- [3) Get the Domain Controllers Private IP](#3-get-the-domain-controllers-private-ip-address)
- [4) Attach Client to Domain Controller](#4-attach-the-client-virtual-machine-to-the-domain-controller)
- [5) Log into the Virtual Machines](#5-log-into-the-virtual-machines)
- [6) Install Active Directory Domain Services](#6-install-active-directory-domain-services)
- [7) Promote Server to Domain Controller](#7-promote-server-to-domain-controller)
- [8) Login to the Client Virtual Machine as the Domain Administrator](#8-login-to-the-client-virtual-machine-as-the-domain-administrator)
- [9) Verify Domain Functionality](#9-verify-domain-functionality)
- [10) Enable Remote Dial-In](#10-enable-remote-dial-in-for-non-administrative-users)
- [11) Give Remote Desktop Permissions](#11-give-remote-desktop-permissions-to-domain-users)
- [12) Verify VM Connectivity](#12-verify-the-virtual-machines-are-connected)
- [13) Create Organizational Units](#13-create-organizational-units)
- [14) Create a User](#14-create-a-user)
- [15) Create Security Groups](#15-create-security-groups)
- [16) Assign Folder Permissions](#16-assign-folder-permissions)
- [17) Attempt Folder Access](#17-attempt-to-access-the-folders)
- [18) Upgrade User to Domain Admin](#18-upgrade-user-to-domain-admin)
- [19) Verify Admin Access](#19-verify-admin-access)
- [20) Create Password Policy](#20-create-an-account-password-policy)
- [21) Lockout User Account](#21-lockout-the-users-account)
- [22) Unlock User Account](#22-unlock-the-users-account)
- [23) Reset User Password](#23-reset-the-users-password)
- [24) Verify Functionality](#24-verify-functionality)
- [25) Set Account Logon Hours](#25-set-account-logon-hours)
- [26) Deactivate Account](#26-deactivating-user-accounts)
- [27) Deprovision Account](#27-deprovisioning-user-accounts)

---

### 1) Create Virtual Machines

1. Open **Microsoft Azure** then search **Resource Groups** and select **create** give the **Resource Group** the following settings then create the **Resource Group**
  - **Name:** RG-01
  - **Reigon:** South Central US

<p align="center">
  <img width="426" height="373" alt="Untitled Diagram-Page-1 drawio" src="https://github.com/user-attachments/assets/a69171bc-fb18-4e60-a6c6-fd4aa9844675" />
</p>

2. Search **Virtual Network** and select **create** give the **Virtual Network** the following settings then create the **Virtual Network**
  - **Resource Group:** RG-01
  - **Name:** VNet-01
  - **Reigon:** South Central US

<p align="center">
  <img width="550" height="612" alt="Untitled Diagram-Page-2 drawio" src="https://github.com/user-attachments/assets/e9d0699e-cf47-4e10-be8f-44c13fcf49b2" />
</p>

3. Seach **Virtual Machines** then **create** give the **Virtual Machine** the following settings then create the **Virtual Machine**
  - **Basics**
    - **Resource Group:** RG-01
    - **Name:** DC-01
    - **Image:** Windows Server 2022
    - **Size:** 2vcpus
    - **Username:** userryan
    - **Password:** Cyberlab123!
  - **Networking**
    - **Virtual Network:** VNet-01
   
<p align="center">
  <img width="609" height="463" alt="Untitled Diagram-Page-3 drawio" src="https://github.com/user-attachments/assets/2686cebb-85f3-4324-bdd6-f620d63f07d6" />
  <img width="410" height="226" alt="Untitled Diagram-Page-4 drawio" src="https://github.com/user-attachments/assets/8f098656-6478-4fd0-b4ba-4dc3d1cbb35c" />
</p>

  4. Seach **Virtual Machines** then **create** give the **Virtual Machine** the following settings then create the **Virtual Machine**
  - **Basics**
    - **Resource Group:** RG-01
    - **Name:** Client-1
    - **Image:** Windows 10 Pro
    - **Size:** 2vcpus
    - **Username:** userryan
    - **Password:** Cyberlab123!
  - **Networking**
    - **Virtual Network:** VNet-01
   
<p align="center">
  <img width="615" height="452" alt="Untitled Diagram-Page-5 drawio" src="https://github.com/user-attachments/assets/3913cb34-0083-4916-8d24-f92ef378a0fa" />
  <img width="418" height="232" alt="Untitled Diagram-Page-6 drawio" src="https://github.com/user-attachments/assets/c038b7e5-79bf-4bf3-82c3-6a0765eeb79e" />
  <img width="1329" height="326" alt="Capture7" src="https://github.com/user-attachments/assets/da033595-cf92-432e-bb1a-daed902f584a" />
</p>

---

### 2) Make the Domain Controller's IP Address Static

1. Select the **DC-01 (Domain Controller)** then select **Network Settings** then open the **Network Interface**

<p align="center">
  <img width="1626" height="533" alt="Drawing3 drawio" src="https://github.com/user-attachments/assets/3d2ec7e6-8b41-4d2c-8082-577b8f82d426">
</p>

2. Select **ipconfig1**
3. For Private IP address setting choose **Static** and save changes

<p align="center">
  <img width="1870" height="762" alt="Drawing4 drawio" src="https://github.com/user-attachments/assets/b978ad18-23b4-420e-8dec-4edf7962d113" />
</p>


---

### 3) Get the Domain Controller's Private IP Address

1. Select the **Domain Controller** then open the **Network Settings** tab and find the **Private IP Address**

<p align="center">
  <img width="1625" height="531" alt="Untitled Diagram drawio" src="https://github.com/user-attachments/assets/54fcc4ca-6b89-4fc8-9dfa-ab3795b47ce0" />
</p>

---

### 4) Attach the Client Virtual Machine to the Domain Controller

1. Select **Client-1 (client virtual machine)** then select **Network Settings** and open the **Network Interface**
2. Select **DNS Servers** and choose **Custom**
3. Enter the **DC's Private IP address** and save

<p align="center">
  <img width="1384" height="629" alt="Drawing5 drawio" src="https://github.com/user-attachments/assets/50e20109-3927-4b33-a3c9-eca327854bc1" />
</p>

4. Restart the client VM

---

### 5) Log into the Virtual Machines

1. Search **Virtual Machines** and check under the **Public IP address** tab for the **Domain Controller's Public IP address** and copy it

<p align="center">
  <img width="1245" height="291" alt="Untitled Diagram-Page-8 drawio (1)" src="https://github.com/user-attachments/assets/cbdbf438-90dd-4f9f-950f-db55e4761e05" />
</p>

2. In the **Windows search bar** search **RDP** to open the **Remote Desktop Protocol**
3. Where it says **Computer** paste the **Domain Controller's Public IP address**

<p align="center">
  <img width="405" height="250" alt="Capture11" src="https://github.com/user-attachments/assets/5b4dfe75-9db2-4afd-b32c-daa4a4520033" />
</p>

4. When it asks for the login credentials enter:
  - **Username:** userryan
  - **Password:** Cyberlab123!

<p align="center">
  <img width="453" height="469" alt="Capture12" src="https://github.com/user-attachments/assets/e0d101ab-a6b2-4af3-a8e1-e90dcbd21fd9" />
</p>

5. To log into the **client virtual machine** copy the **client's public IP address** and follow the same steps

---

### 6) Install Active Directory Domain Services

1. In the **Domain Controller** and open the **Server Manager** then select **Add roles and features**
2. On the **Server Roles** tab check **Active Directory Domain Services** then complete the installation

<p align="center">
  <img width="783" height="558" alt="Drawing6 drawio (1)" src="https://github.com/user-attachments/assets/501c6406-cc17-46e2-9f33-6abc957f1de4" />
</p>

---
### 7) Promote Server to Domain Controller

1. In the **Server Manager** click the **notification flag** and select **Promote this server to a domain controller**

<p align="center">
  <img width="1921" height="784" alt="Drawing7 drawio" src="https://github.com/user-attachments/assets/1dde3344-4dd4-4051-bf4a-c71120f3a326" />
</p>

2. Choose **Add a new forest** and set the root domain name to **domain.name**

<p align="center">
  <img width="759" height="556" alt="Drawing8 drawio" src="https://github.com/user-attachments/assets/1fff2c8d-80e7-4574-98f3-9bea980308ef" />
</p>

3. Set the Directory Services Restore Mode (DSRM) password to **Cyberlab123!** and complete the install and reboot the VMM

---

### 8) Login to the Client Virtual Machine as the Domain Administrator

1. Open the **Remote Desktop Protocol** and enter the **client virtual machine's public IP address**
2. When asked about the username name and password enter
   - **Username:** domain.name\userryan
   - **Password:** Cyberlab123!
  
<p align="center">
  <img width="454" height="468" alt="Capture2" src="https://github.com/user-attachments/assets/b6ca06fd-0af3-46c0-b0ef-b2fb9e2f9823" />
</p>

---

### 9) Verify Domain Functionality

1. Log into the **Client Virtual Machine** as the **Domain Administrator** open **Windows PowerShell as an Administrator**
2. Attempt to ping the DC's private IP address using the command **`ping 10.0.0.4`**
  - Your private IP address is most likely different. If you don't remember return to [3) Get the Domain Controllers Private IP](#3-get-the-domain-controllers-private-ip-address)
3. Ensure the ping succeeded

<p align="center">
  <img width="858" height="396" alt="Drawing9 drawio" src="https://github.com/user-attachments/assets/e726fac9-d102-4c9b-93f6-c5e6f93eda7d" />
</p>

4. Enter the command **`ipconfig /all`** into Windows Powershell
5. Confirm the output for the client's DNS settings shows the DC's private IP address

<p align="center">
  <img width="859" height="561" alt="Drawing10 drawio" src="https://github.com/user-attachments/assets/cb116758-0992-466c-9230-885a7b8a4ebc" />
</p>

---

### 10) Enable Remote Dial-In for Non-Administrative Users

1. In the **Client Virtual Machine** right click the **Start Button** and select **System**
2. Navigate to the **About** page and select **Rename this PC (advanced) then click **Change**
3. Check the **Member of Domain** box and enter the name of the **domain** and apply the changes

<p align="center">
  <img width="1638" height="1079" alt="Previous1 drawio (1)" src="https://github.com/user-attachments/assets/490a7c5f-43c2-4692-836e-791d75116410" />
</p>

---

### 11) Give Remote Desktop Permissions to Domain Users

1. On the **Client Virtual Machine** right click the **Start Button** and select **Computer Management**
2. Go to **Local Users and Groups** and open the **Groups** folder
3. Select **Remote Desktop Users** and click **Add**
4. Type **Domain Users** in the box and click **Check Names**
5. Apply the changes

<p align="center">
  <img width="1315" height="835" alt="Untitled Diagram drawio" src="https://github.com/user-attachments/assets/0e47978d-ae31-48e5-b0ee-769e49cf6ae9" />
</p>

9. Restart the VM

---

### 12) Verify the Virtual Machines are Connected

1. Open the **Server Manager** on the **Domain Controller**
2. Select **Tools** then **Active Directory Users and Computers**

<p align="center">
  <img width="1921" height="746" alt="Previous2 drawio (2)" src="https://github.com/user-attachments/assets/3a758583-b630-4971-839f-a02f0c974cab" />
</p>

3. Expand the **Domain** then click **Computers**
4. The client VM should be inside

<p align="center">
  <img width="754" height="529" alt="Previous3 drawio" src="https://github.com/user-attachments/assets/8ce7e5f3-34c7-4138-98ce-e09fe37048f9" />
</p>

---

### 13) Create Organizational Units

1. Open the **Server Manager** on the **Domain Controller**
2. Select **Tools** then **Active Directory Users and Computers**

<p align="center">
  <img width="1920" height="759" alt="Untitled Diagram-Page-2 drawio" src="https://github.com/user-attachments/assets/e288e202-d97b-42fb-bb4e-5658a4e9fe63" />
</p>

3. Right click the **domain**
4. Open the **New** submenu
5. Then select **Organizational Unit**

<p align="center">
  <img width="755" height="531" alt="Untitled Diagram-Page-3 drawio" src="https://github.com/user-attachments/assets/8635d7b6-7680-4189-af90-ba22229373c2" />
</p>

6. Create 3 Organizational Units called: Employees, Admins, and Groups

<p align="center">
   <img width="754" height="530" alt="Untitled Diagram-Page-4 drawio" src="https://github.com/user-attachments/assets/df1a2b65-9fc2-439f-be2b-fbd7fc110984" />
</p>

---

### 14) Create a User

1. Right click the **Employees** folder
2. Open the **New** submenu
3. Then seelect **User**

<p align="center">
   <img width="755" height="549" alt="Untitled Diagram-Page-5 drawio" src="https://github.com/user-attachments/assets/62185a90-7adb-4ac9-964c-d1c3bcc2e80b" />
</p>

4. Name the user **Ryan Kennon**

<p align="center">
   <img width="753" height="530" alt="Untitled Diagram-Page-6 drawio" src="https://github.com/user-attachments/assets/80ff677e-49a9-486f-b59a-6d47cf2066cb" />
</p>

---

### 15) Create Security Groups

1. Open the **Groups** folder
2. Open the **New** submenu
3. Then select **Group**

<p align="center">
   <img width="753" height="583" alt="Untitled Diagram-Page-21 drawio" src="https://github.com/user-attachments/assets/ecfd98c1-aad7-4f23-8f6a-71b54da52b74" />
</p>

4. Name the Group: Human Resources

<p align="center">
   <img width="754" height="530" alt="Untitled Diagram-Page-7 drawio" src="https://github.com/user-attachments/assets/7cdc7f08-cf95-444c-a7ac-472701c48984" />
</p>

5. Double click the **Human Resources** security group
6. Select **Members** then select **Add**
7. Enter the name of the user then **Check Names**
8. Apply the changes

<p align="center">
   <img width="1056" height="562" alt="Untitled Diagram-Page-8 drawio" src="https://github.com/user-attachments/assets/e40e3052-d247-4519-846b-0f13e256127f" />
</p>

---

### 16) Assign Folder Permissions

1. On the **`C: \`** create 3 folders named: HR-ReadWrite, HR-ReadOnly, and AdminsOnly

<p align="center">
   <img width="1125" height="633" alt="Untitled Diagram-Page-9 drawio" src="https://github.com/user-attachments/assets/bc89fd33-3fc2-48fc-b23d-d1200368f695" />
</p>

2. Open the **Properties** for the folder called **HR-ReadWrite**
3. Then select **Sharing** then **Share**

<p align="center">
   <img width="361" height="479" alt="Untitled Diagram-Page-10 drawio" src="https://github.com/user-attachments/assets/4997118c-879d-4ced-b7b3-a6c14a83a9fa" />
</p>

4. Then enter **Human Resources** in the box then select **Add**
5. Then click the **dropdown arrow** and select **Read/Write**
6. Confirm the changes

<p align="center">
   <img width="613" height="453" alt="Untitled Diagram-Page-11 drawio" src="https://github.com/user-attachments/assets/e5840ca3-0b27-43e3-a59f-b463ae41941a" />
</p>

7. Do the same for the **HR-ReadOnly** folder except give the Human Resources group **Read** priveleges only.

<p align="center">
   <img width="613" height="454" alt="Untitled Diagram-Page-12 drawio" src="https://github.com/user-attachments/assets/ce474a2d-0785-4e4e-9427-79ebdd7916bf" />
</p>

8. For the **AdminsOnly** folder, enter **Domain Admins** in the box before hitting **Add**
9. Select **Read/Write** priveleges for the **Domain Admins**
10. Apply the changes

<p align="center">
   <img width="613" height="453" alt="Untitled Diagram-Page-13 drawio" src="https://github.com/user-attachments/assets/5bb94385-5358-423c-b51f-3639bbb3d45e" />
</p>

---

### 17) Attempt to Access the Folders

1. **Log into the client VM** using the **credentials of the user** created earlier
2. Open the **File Explorer**
3. On the **Quick Access** bar search **`\\<DC name>`**

<p align="center">
   <img width="1124" height="633" alt="Untitled Diagram-Page-14 drawio" src="https://github.com/user-attachments/assets/81babfc6-144c-4ffa-b10c-225d43e2b849" />
</p>

4. Attempt to access the **HR-ReadWrite** folder and create a new file inside

<p align="center">
   <img width="1125" height="634" alt="Untitled Diagram-Page-15 drawio" src="https://github.com/user-attachments/assets/0ca74ee8-c7da-4869-a518-5c0010cd411d" />
</p>

5. Attempt to access the **HR-ReadOnly** folder and attempt to create a new file inside

<p align="center">
   <img width="1123" height="630" alt="Untitled Diagram-Page-16 drawio" src="https://github.com/user-attachments/assets/eb2b72df-bfb2-41a2-8823-0012887f433a" />
</p>

6. Attempt to access the **NoAccess** folder

<p align="center">
   <img width="1122" height="630" alt="Untitled Diagram-Page-17 drawio" src="https://github.com/user-attachments/assets/a09c2962-6d55-4b04-b54d-1fb3bd0ccdf4" />
</p>

---

### 18) Upgrade User to Domain Admin

1. Go back to **Active Directory Users & Computers** on the **Domain Controller**
2. Open the **Employees** folder then right click the user **Ryan Kennon** and select **Properties**

<p align="center">
   <img width="751" height="527" alt="Untitled Diagram-Page-20 drawio" src="https://github.com/user-attachments/assets/a7fb0f02-e418-43b2-96e2-0f246cdf8c54" />
</p>

3. Select **Member Of** then **Add**
4. Type **Domain Admin**
5. Then **Check Names**
6. Confirm the changes

<p align="center">
   <img width="653" height="536" alt="Untitled Diagram-Page-18 drawio" src="https://github.com/user-attachments/assets/ff9dd259-2533-40d6-bbcb-cbfba16e531b" />
</p>

---

### 19) Verify Admin Access

1. Log back in to the **client VM** using the **user's credentials**
2. Search **`\\DC-01`** in the **Quick Access** bar again
3. Attempt to open the **AdminsOnly** folder and attempt to create a new file inside

<p align="center">
   <img width="1122" height="631" alt="Untitled Diagram-Page-19 drawio" src="https://github.com/user-attachments/assets/ad515c8e-0afe-4bd9-a4f6-4e9a72968a15" />
</p>

---

### 20) Create an Account Password Policy

1. On the **Domain Controller** open the **Server Manager**
2. Select **Tools** then **Group Policy Management**

<p align="center">
  <img width="1310" height="761" alt="Untitled Diagram-Page-1 drawio" src="https://github.com/user-attachments/assets/6ebebf36-19ee-4cc6-9acc-ec17be43ade2" />
</p>

3. Navigate through the **Forest** to the **Default Domain Policy**
4. Right click **Default Domain Policy** then choose **Edit**

<p align="center">
  <img width="752" height="527" alt="Untitled Diagram-Page-2 drawio" src="https://github.com/user-attachments/assets/3e2e4023-c338-4864-af0f-fff899244cdf" />
</p>

5. In the **Group Policy Management Editor** navigate through the **Policies** folder to the **Password Policy**

<p align="center">
  <img width="785" height="562" alt="Untitled Diagram-Page-3 drawio" src="https://github.com/user-attachments/assets/e6fb7b79-9cc3-4ea4-8c29-2a662c646909" />
</p>

6. Change the **Maximum Password Age** to **30 days**

<p align="center">
  <img width="715" height="589" alt="Untitled Diagram-Page-4 drawio" src="https://github.com/user-attachments/assets/b7de2388-821e-46f2-9d0c-002d54b39ac1" />
</p>

7. Change the **Minimum Password Length** to **12 characters**

<p align="center">
  <img width="1019" height="668" alt="Untitled Diagram-Page-5 drawio" src="https://github.com/user-attachments/assets/06b4eb15-f0e0-4b93-9bf4-c4b4e3740da5" />
</p>

8. Open the **Account Lockout Policy**
9. Change the **Account Lockout Threshold** to **3 invalid login attempts**

<p align="center">
  <img width="1022" height="669" alt="Untitled Diagram-Page-6 drawio" src="https://github.com/user-attachments/assets/c934b2ba-62f9-4d48-aaa0-c108e08c3784" />
</p>

10. Change the **Account Lockout Duration** to **360 minutes**

<p align="center">
  <img width="713" height="584" alt="Untitled Diagram-Page-7 drawio" src="https://github.com/user-attachments/assets/3a9d850d-33b0-43e9-91fd-48c919d9d68f" />
</p>

11. Go back to the **Group Policy Management** page
12. Right-click **Default Domain Policy**
13. Click **Enforce**

<p align="center">
  <img width="751" height="527" alt="Untitled Diagram-Page-8 drawio" src="https://github.com/user-attachments/assets/af525589-f358-4b69-9ae8-9f9a0a696945" />
</p>

---

### 21) Lockout the User's Account

1. Attempt to log into the **client Virtual Machine** using an **incorrect password** four times

<p align="center">
  <img width="567" height="369" alt="Untitled Diagram-Page-9 drawio" src="https://github.com/user-attachments/assets/888ab766-b79a-4c76-b220-5e1fe7899df0" />
</p>

---

### 22) Unlock the User's Account

1. Open **Active Directory Users and Computers**
2. Double click the user **Ryan Kennon**
3. Click **Account**
4. Check the box labeled **Unlock Account**
5. Apply the Changes

<p align="center">
  <img width="749" height="667" alt="Untitled Diagram-Page-10 drawio" src="https://github.com/user-attachments/assets/35276247-79bc-427e-a979-62ebee8d1975" />
</p>

---

### 23) Reset the User's Password

1. Right click the user **Ryan Kennon**
2. Select **Reset Password**

<p align="center">
  <img width="751" height="525" alt="Untitled Diagram-Page-11 drawio" src="https://github.com/user-attachments/assets/71c01277-8ad5-4fb3-8328-124e95e3b0f1" />
</p>

3. Enter the new password
4. Apply the Changes

<p align="center">
  <img width="377" height="254" alt="Untitled Diagram-Page-12 drawio" src="https://github.com/user-attachments/assets/8065b96f-2809-4d0c-b42a-bda8e3843270" />
</p>

---

### 24) Verify Functionality

1. Attempt to log into the **client Virtual Machine** using the updated **user credentials**

<p align="center">
  <img width="786" height="444" alt="Untitled Diagram-Page-13 drawio" src="https://github.com/user-attachments/assets/bee7a8f1-ecee-4678-b7f7-63e19546e757" />
</p>

---

### 25) Set Account Logon Hours

1. On the **Domain Controller** open **Active Directory Users and Computers**
2. Right-click the user **Ryan Kennon** and select **Properties**

<p align="center">
  <img width="752" height="528" alt="Untitled Diagram-Page-1 drawio" src="https://github.com/user-attachments/assets/db0724d4-87f1-47f1-8ca4-0720ae61eb45" />
</p>

3. Navigate to the **Account** tab and click **Logon Hours**
4. Select **Logon Denied** to clear the hours
5. Apply the changes

<p align="center">
  <img width="628" height="556" alt="Untitled Diagram-Page-2 drawio" src="https://github.com/user-attachments/assets/5be9bab1-b54a-4207-bb65-f32309912c64" />
</p>

6. Attempt to log into the **Client Virtual Machine** using the **User's Credentials** to observe the change

<p align="center">
  <img width="556" height="356" alt="Untitled Diagram-Page-3 drawio" src="https://github.com/user-attachments/assets/956ffec0-bf21-402a-905c-6485c23f2263" />
</p>

7. On the **Logon Hours** page highlight all the hours and select **Logon Permitted** and apply the changes to reenable sign on

<p align="center">
  <img width="504" height="319" alt="Untitled Diagram-Page-4 drawio" src="https://github.com/user-attachments/assets/8f4db8fa-7001-423e-a5a5-f1a1a6b393c5" />
</p>

---

### 26) Deactivating User Accounts

1. In **Active Directory Users and Computers** right-click the user **Ryan Kennon**
2. Select **Disable Account**

<p align="center">
  <img width="751" height="527" alt="Untitled Diagram-Page-5 drawio" src="https://github.com/user-attachments/assets/e40b9962-228c-49bb-9297-62e788293db2" />
</p>

3. Attempt to log into the **Client Virtual Machine** using the **User's Credentials** to observe the change

<p align="center">
  <img width="554" height="353" alt="Untitled Diagram-Page-6 drawio" src="https://github.com/user-attachments/assets/84c46e04-3b9f-4761-92ab-1dc62f5c38e4" />
</p>

4. In **Active Directory Users and Computers** right-click the user
5. Select **Enable Account** to reactive the user account

<p align="center">
  <img width="751" height="528" alt="Untitled Diagram-Page-7 drawio" src="https://github.com/user-attachments/assets/729d3251-ebae-41c6-a384-0f335e9e375c" />
</p>

---

### 27) Deprovisioning User Accounts

1. In **Active Directory Users and Computers** right-click the user **Ryan Kennon**
2. Select **Delete**
3. Confirm you want to delete the user

<p align="center">
  <img width="752" height="528" alt="Untitled Diagram-Page-8 drawio" src="https://github.com/user-attachments/assets/12ae387a-88d9-41ee-ae40-8a47b0962789" />
</p>

4. Attempt to log into the **Client Virtual Machine** using the **User's Credentials** to observe the change

<p align="center">
  <img width="452" height="415" alt="Untitled Diagram-Page-9 drawio" src="https://github.com/user-attachments/assets/0d5ed55c-72fc-4a9b-9e6d-142a3c1fbda0" />
</p>
