# Password and Account Lockout Policy
Testing out Group Policies that attempt to strengthen company resistence to account breaches. This is done by setting up strict rules within the Password Policy and Account Lockout Policy for users and devices on this domain

# Key Tech
 - Proxmox Virtual Environment 
 - Windows Server AD DC VM
 - Windows 11 Client VM
 - Powershell Administrator
 - Group Policy Management
 - Account Policies

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


![Account Policy](Account%20Policy.png)
![Account Lockout Policy](Account%20Lockout%20Policy.png)

# Updating to Computers on Domain


![Force Update](Force%20Update.png)
![Force Update Success](Force%20Update%20Successful.png)

# Testing Lockout Policy


![User Lockedout](User%20Locked%20Out%202.png)

# Unlocking Account


![Unlocking Account](Unlocking%20Account.png)

# Testing Unlocked Account


![Login Success](Login%20Successful%201.png)

![Login Success](Login%20Successful%202.png)
