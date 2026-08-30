# Password and Account Lockout Policy
This lab documents the implementation of Group Policy Objects (GPOs) designed to strengthen domain resistance against unauthorized access. The project focuses on deploying strict password complexity rules and account lockout mechanisms to protect both user accounts and domain-joined devices.

# Key Tech
* **Hypervisor**: Proxmox Virtual Environment (PVE)
* **Directory Services**: Active Directory Domain Services (AD DS)
* **Management Tools**: Group Policy Management Console (GPMC), Active Directory Users and Computers (ADUC)
* **Automation/CLI**: Elevated PowerShell (Admin)
* **Operating Systems**: Windows Server 2022, Windows 11 Enterprise

# Proxmox Virtual Environment
| Machine | Role | IP Address |
|---|---|---|
| Windows Server AC DC VM | Domain Controller | `192.168.1.111` |
| Windows 11 Client VM | Domain-Joined Client | `192.168.1.112` |

*Wins 11 Client was joined to the domain via using the Wins Server IP as its DNS*

### WS Config

![WS Config](../Role%20Based%20Access%20Control/WS%20Config.png)

### W11 Config

![W11 Config](../Role%20Based%20Access%20Control/W11%20Config.png/)

# Domain Server


![Domain Server](Domain%20Server.png)

# Account Policy
Using the Group Policy Management Editor, I hardened domain authentication by updating the Password and Account Lockout Policies. Key configurations include a 31-day password age limit, a minimum password length of 7 characters with complexity enabled, and an Account Lockout Threshold set to 3 attempts, requiring manual administrator unlock

![Account Policy](Account%20Policy.png)
![Password Policy](Password%20Policy.png)
![Account Lockout Policy](Account%20Lockout%20Policy.png)

# Group Policy Update
To enforce the new policies immediately, I ran gpupdate /force in an elevated PowerShell console, forcing a domain-wide Group Policy refresh and bypassing the default background propagation delay

![Force Update](Force%20Update.png)
![Force Update Success](Force%20Update%20Successful.png)

# Updating Password
Enforcement of the password policy was verified through user-side testing. An initial attempt to update a domain account with a weak password failed due to complexity and length checks. A subsequent attempt using a strong credential succeeded, validating that the Group Policy restrictions are successfully active on domain endpoints

![User Changing Password](User%20Changing%20Password.png)
![User Changing Password 2](User%20Changing%20Password%202.png)
![Bad Password](Bad%20New%20Password.png)
![Unsuccessful Password Change](Unsuccessful%20Password%20Change.png)
![Good Password](Good%20New%20Password.png)
![Successful Password Change](Successful%20Password%20Change.png)

# Testing Lockout Policy
Account Lockout Policy enforcement was verified by simulating a brute-force attack on a domain account. Following the third consecutive failed login attempt, the account was immediately locked out, validating the 'three-strike' threshold restriction and confirming protection against automated credential attacks

![User Lockedout](User%20Locked%20Out%202.png)

# Unlocking Account
Account remediation was completed by checking the 'Unlock account' flag within ADUC to restore user access. To distinguish this from a true brute-force event, enterprise security protocols require a comprehensive incident response workflow: out-of-band identity verification (Direct phone call/In Person), MFA session revocation, a forced password reset, and a monitored account unlock

![Unlocking Account](Unlocking%20Account.png)

# Testing Unlocked Account
Once the password was reset and the account unlocked, user authentication succeeded, verifying the complete remediation of the lockout state

![Login Success](Login%20Successful%201.png)
![Login Success](Login%20Succesful%202.png)
