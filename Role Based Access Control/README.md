# Role Based Access Control
Implemented a Role-Based Access Control framework within a Windows Server Active Directory (AD DC) environment to enforce data separation and department-level security compliance
 - Automated Provisioning: Developed and executed a PowerShell script to bulk-provision 100 user accounts
 - Architecture: Structured the AD environment by designing dedicated Organizational Units (OUs) and Security Groups to mirror corporate departmental divisions
 - Access Control & File Security: Standardized data access by creating departmental shared folders governed by strict NTFS and Share permissions
 - Validation & Testing: Verified the security posture by deploying a Windows 11 Client VM to conduct cross-departmental access testing, ensuring proper permission inheritance and restriction enforcement

# Key Tech
 - Proxmox Virtual Environment 
 - Windows Server AD DC VM
 - Windows 11 Client VM
 - Powershell Administrator
 - Share and NTFS Permissions
 - Role Based Access Control

# Proxmox Virtual Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server AC DC VM | Domain Controller | `192.168.1.111` |
| Windows 11 Client VM | Domain-Joined Client | `192.168.1.112` |

*Wins 11 Client was joined to the domain via using the Wins Server IP as its DNS*

### WS Config

![WS Config](WS%20Config.png)

### W11 Config

![W11 Config](W11%20Config.png)

# Domain Controller and Active Directory Setup
After configuring the virtual machine, I booted the Windows Server ISO and completed the operating system installation. Upon reaching the desktop, I assigned a static IP address, configured the DNS settings to point to the local loopback address, installed the Active Directory Domain Services (AD DS) role, and promoted the server to a Domain Controller

![Domain Controller](Domain%20Controller.png)

# Bulk User Provision
To simulate a business environment, I developed a automated script that generates 100 new users using a incremental for loop placing them all into a general "Staff" OU

![Bulk Provision Script](Bulk%20User%20Add%20Script.png)

# Department Based OUs and Security Groups
I created IT, Accounting, and Legal sub-OUs to separate the new user accounts by department, along with respective security groups to implement Role-Based Access Control (RBAC)

![Departments OUs](Staff%20OU.png)
![IT OU](IT%20OU.png)
![Accounting OU](Accounting%20OU.png)
![Legal OU](Legal%20OU.png)
![IT SG](IT%20SG.png)
![Accounting SG](Accounting%20SG.png)
![Legal SG](Legal%20SG.png)

# Shared Department Folders and Permissions
To implement RBAC, I provisioned local departmental folders and created network shares with restricted visibility based on group membership. I then configured NTFS security permissions on each folder to grant the respective groups file access and modification rights

![Shared Folders](Shared%20Folders.png)
![Accounting Folder Network Path](Accounting%20Folder%20Properties.png)
![Share Permissions](Accounting%20Share%20Permissions.png)
![NTFS Permissions](Accounting%20NTFS%20Permissions.png)

# Access Test
To see if my permissions were successful I logged into the Windows 11 Client VM using one of the accounting users information. I attempted to map the Accounting Shared Folder using the network path and it was successful. I could read, write, and delete files within that folder. To test if I was able to access the other department folders, I typed in the IT's network path and was stopped as expected, preventing users from accessing information outside of their role

![Mapping Drive](Mapping%20Accounting%20Shared%20Drive.png)
![Drive Successfully Mapped](Mapping%20Accounting%20Shared%20Drive%202.png)
![Empty Drive](Mapping%20Accounting%20Shared%20Drive%203.png)
![IT Folder Denied Access](IT%20Shared%20Folder%20Permission%20Denied.png)
