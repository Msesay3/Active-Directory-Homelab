# Role Based Access Control
Implemented a Role-Based Access Control framework within a Windows Server Active Directory (AD DC) environment to enforce data segregation and department-level security compliance.
 - Automated Provisioning: Developed and executed a PowerShell script to bulk-provision 100 user accounts
 - Architecture: Structured the AD environment by designing dedicated Organizational Units (OUs) and Security Groups to mirror corporate departmental divisions
 - Access Control & File Security: Standardized data access by creating departmental shared folders governed by strict NTFS and Share permissions
 - Validation & Testing: Verified the security posture by deploying a Windows 11 Client VM to conduct cross-departmental access testing, ensuring proper permission inheritance and restriction enforcement.

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
![W11 Config](WS11%20Config.png)
