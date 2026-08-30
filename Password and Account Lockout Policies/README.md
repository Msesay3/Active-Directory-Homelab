# Password and Account Lockout Policies Lab
This lab documents the implementation of Group Policy Objects (GPOs) designed to strengthen domain resistance against unauthorized access. The project focuses on deploying strict password complexity rules and account lockout mechanisms to protect both user accounts and domain-joined devices.

## Key Technologies & Environment
* **Hypervisor**: Proxmox Virtual Environment (PVE)
* **Directory Services**: Active Directory Domain Services (AD DS)
* **Management Tools**: Group Policy Management Console (GPMC), Active Directory Users and Computers (ADUC)
* **Automation/CLI**: Elevated PowerShell (Admin)
* **Operating Systems**: Windows Server 2025, Windows 11 

### Network Infrastructure
The lab infrastructure consists of two virtual machines hosted within a custom Proxmox environment. The Windows 11 client was joined to the domain by configuring its local network adapter to use the Windows Server's IP address as its primary DNS server.

| Machine | Role | IP Address |
|---|---|---|
| Windows Server AC DC VM | Domain Controller | `192.168.1.111` |
| Windows 11 Client VM | Domain-Joined Client | `192.168.1.112` |

#### WS Config

![WS Config](../Role%20Based%20Access%20Control/WS%20Config.png)

#### W11 Config

![W11 Config](../Role%20Based%20Access%20Control/W11%20Config.png/)

## Account Policy Configuration
Using the **Group Policy Management Editor**, I hardened domain authentication by updating the root Account Policies. Key configurations include:
* **Password Policy**: Mandated a 31-day maximum password age limit, a minimum password length of 7 characters, and enabled strict password complexity requirements.
* **Account Lockout Policy**: Set the **Account Lockout Threshold** to 3 failed attempts (three-strike rule), requiring manual administrator intervention to unlock.

![Password Policy](Password%20Policy.png)
![Account Lockout Policy](Account%20Lockout%20Policy.png)

### Enforcing the Group Policy Update
To enforce the new security baselines immediately, I ran `gpupdate /force` within an elevated PowerShell console on the domain endpoints. This forced a domain-wide Group Policy refresh, completely bypassing the default background propagation delay.

![Force Update Success](Force%20Update%20Successful.png)

---

## Policy Verification & Security Testing

### 1. Password Complexity Testing
Enforcement of the password policy was verified through user-side testing on the Windows 11 Client VM. 
* **Test 1 (Failure State)**: An initial attempt to update a domain account with a basic, weak password failed due to length and complexity checks.
* **Test 2 (Success State)**: A subsequent attempt using a strong, compliant credential succeeded, validating that the Group Policy restrictions are successfully active on domain endpoints.

![Bad Password](Bad%20New%20Password.png)
![Unsuccessful Password Change](Unsuccessful%20Password%20Change.png)
![Good Password](Good%20New%20Password.png)
![Successful Password Change](Successful%20Password%20Change.png)

### 2. Account Lockout Policy Verification
Account Lockout Policy enforcement was verified by simulating a brute-force attack on a domain account from the client machine. Following the third consecutive failed login attempt, the account was immediately locked out, validating the 'three-strike' threshold restriction and confirming active protection against automated credential attacks

![User Lockedout](User%20Locked%20Out%202.png)

---

## Remediation & Incident Response Workflow

### Helpdesk Remediation
Account remediation was completed using **Active Directory Users and Computers (ADUC)**. I located the locked user account, opened its properties, and checked the **'Unlock account'** flag to restore standard access.

![Unlocking Account](Unlocking%20Account.png)

### Security Enterprise Protocol (Suspected Brute-Force Event)
To distinguish a true malicious brute-force attack from a standard accidental lockout, a comprehensive incident response workflow must be initiated prior to checking the unlock box:
1. **Out-of-Band Identity Verification**: Confirm the identity of the user requesting the unlock via an independent network channel (e.g., a direct phone call to their verified personal number or a face-to-face meeting).
2. **MFA Session Revocation**: Terminate and revoke all active Multi-Factor Authentication (MFA) tokens and active web sessions to kick any malicious actors out of active endpoints.
3. **Forced Password Reset**: Change the compromised password to a temporary complex string and check the *"User must change password at next logon"* attribute.
4. **Monitored Account Unlock**: Check the unlock box in ADUC and monitor security logs for subsequent authentication attempts.

### Final Success Testing
Once the password was securely reset and the lockout flag cleared, user authentication succeeded on the Windows 11 client machine, verifying the complete and secure remediation of the lockout state.

![Login Success](Login%20Successful%201.png)
![Login Success](Login%20Succesful%202.png)
