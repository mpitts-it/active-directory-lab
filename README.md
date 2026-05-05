# Active Directory Home Lab

## Project Overview
In this project I deployed Windows Server 2025 as a Domain Controller (DC) in my Proxmox home lab environment. Using Active Directory I created an Organizational Unit (OU) structure, security groups, and user accounts that mirror a real company IT environment. I also configured Group Policy Objects to enforce company wide security controls. To complete the lab I deployed a Windows 10 client virtual machine and joined it to the domain, then simulated real helpdesk scenarios including password resets, account lockouts, account creation, and security incident response.

## Environment
- Hypervisor: Proxmox VE
- Domain Controller OS: Windows Server 2025 Standard Evaluation
- Domain Controller Name: DC01
- Domain Name: mario.local
- Client OS: Windows 10 Pro
- Client Name: WKSTN01
- DC01 Specs: 4 cores, 6GB RAM, 60GB disk
- WKSTN01 Specs: 2 cores, 2GB RAM, 40GB disk

## Skills Demonstrated
- Deployed and configured Windows Server 2025 and Windows 10 virtual machines in Proxmox
- Installed Active Directory Domain Services and promoted the server to a Domain Controller
- Created an Organizational Unit structure including HR, IT, Finance, and Marketing
- Created domain user accounts and security groups including Helpdesk-Staff, Security-Analysts, Finance-Staff, Marketing-Staff, and HR-Staff
- Configured Group Policy including minimum and maximum password length and age, USB storage restriction, and login warning banner
- Troubleshot VM boot issues by identifying and downloading the correct Windows Server 2025 evaluation ISO
- Deployed Windows 10 client VM and installed VirtIO drivers for network and storage
- Joined WKSTN01 to the mario.local domain using domain administrator credentials
- Verified domain connectivity by pinging DC01 by IP and by domain name to confirm DNS resolution
- Simulated helpdesk scenarios including password reset, account lockout, social engineering attack, and unauthorized user creation request

## Active Directory Configuration

### Organizational Structure
I created four top-level Organizational Units to simulate a small scale enterprise environment: Finance, HR, IT, and Marketing. Within the IT OU I created two sub-OUs — Helpdesk and Security — to further organize IT staff by role.

### Users Created
Seven domain user accounts were created across the domain:
- **John Smith** (jsmith) — IT\Helpdesk
- **Sarah Johnson** (sjohnson) — IT\Helpdesk
- **Tom Brady** (tbrady) — IT\Security
- **Jane Wilson** (jwilson) — IT\Security
- **Michael Scott** (mscott) — HR
- **Angela Martin** (amartin) — Finance
- **Jim Halpert** (jhalpert) — Marketing

### Security Groups
Five security groups were created and users assigned based on their role:
- **Helpdesk-Staff** — John Smith, Sarah Johnson
- **Security-Analysts** — Tom Brady, Jane Wilson
- **HR-Staff** — Michael Scott
- **Finance-Staff** — Angela Martin
- **Marketing-Staff** — Jim Halpert

This structure was designed to simulate having to locate users across different departments and understand the separation between OUs and Security Groups in a real enterprise environment.

## Group Policy Configuration

All policies were configured in the Default Domain Policy to ensure company wide enforcement across all users and computers in the mario.local domain.

### Password Policy
- **Minimum password length:** 14 characters — enforces strong passwords that are difficult to guess or crack
- **Maximum password age:** 90 days — ensures passwords are regularly changed to reduce the risk of stolen credentials being used long term
- **Password history:** 24 passwords remembered — prevents users from cycling back to previously used passwords

### Account Lockout Policy
- **Lockout threshold:** 5 failed attempts — prevents brute force attacks by locking the account after repeated failed login attempts
- **Lockout duration:** 30 minutes — acts as a deterrent while not permanently locking out legitimate users
- **Reset counter:** 10 minutes — resets the failed attempt counter after inactivity

### Login Warning Banner
Configured a security notice that displays before every login informing users that the system is for authorized users only and that all activity is monitored. This serves as both a deterrent and legal documentation that users acknowledged the terms before accessing the system.

### USB Storage Restriction
Disabled read, write, and execute access to removable storage devices across all domain computers. This prevents users from introducing malware via USB drives and stops unauthorized copying of corporate data to external devices.

## Helpdesk Scenarios Simulated

### Scenario 1 — Password Reset and Account Unlock
A user called reporting their account was locked after multiple failed login attempts upon returning from vacation. They had an important meeting in 20 minutes adding time pressure to the situation. The user was authenticated, a ticket was opened, and it was confirmed they had forgotten their password. The account was unlocked and a temporary password was issued via phone call. The user was required to change their password at next logon. Successful login was verified before the ticket was closed and documented.

### Scenario 2 — Account Lockout
A user called reporting their account was locked out. Identity was verified, a ticket was opened, and the account was unlocked in Active Directory Users and Computers. The resolution was documented and the ticket was closed.

### Scenario 3 — Social Engineering Attack
A member of the security team called requesting that a colleague's account be disabled immediately claiming a potential compromise. Rather than acting on the request immediately the caller was authenticated and a ticket was opened. The situation was escalated to the security manager for verification. It was determined that the caller was actually the insider threat. The caller's account was disabled, everything was documented, and the incident was escalated to the security manager for further investigation into insider threat activity.

### Scenario 4 — Unauthorized User Creation Request
An unknown caller requested that a new employee account be created. The caller was asked to identify themselves and authenticate using their company ID. Before taking any action it was confirmed whether a formal HR onboarding request existed. HR was contacted directly to verify the new hire was legitimate. A formal written request was required before the account was created. All steps were documented and the ticket was closed upon completion. This scenario demonstrated that account creation requests must follow a formal authorization process to prevent unauthorized accounts from being created through social engineering.

## Challenges and Troubleshooting

**Wrong ISO File**
One of the main challenges I encountered was downloading the wrong ISO file for Windows Server 2025. Without reading carefully I downloaded the Features on Demand ISO instead of the installation ISO. This caused repeated VM boot failures across multiple configuration attempts. After extensive troubleshooting of BIOS settings, machine types, and storage controllers, I discovered the root cause was the source file itself. This taught me to always verify software requirements and read documentation carefully before beginning an installation — checking the source before troubleshooting the configuration.

**Understanding OUs vs Security Groups**
I initially struggled with the distinction between Organizational Units and Security Groups in Active Directory. Through research and hands on configuration I learned that OUs are containers that organize where accounts live and where policies are applied, while Security Groups control what resources users can access. Understanding this distinction is fundamental to properly administering Active Directory.

**Driver Installation**
Learning how to install VirtIO drivers on both Windows Server and Windows 10 was a new experience. Windows does not natively recognize VirtIO devices so drivers had to be manually loaded during installation for the SCSI controller and after installation for the network adapter and other devices. This gave me practical experience with driver management which is a common helpdesk task.

## What I Learned

I learned a lot during this project from creating a Domain Controller to setting Group Policies. What surprised me was how straightforward Windows Server is to navigate once you understand the structure — creating and managing Organizational Units, security groups, users, and devices through the GUI is very intuitive. I wanted to do this project to reinforce my Active Directory knowledge and gain hands on experience because I was familiar with it from school but knew I needed practical exposure to truly understand it.

This project showed me how much control IT departments have over an organization and how many opportunities there are to disrupt business operations if something is done incorrectly. That realization made me appreciate why change management, documentation, and proper authorization processes exist — one wrong click in Active Directory can affect hundreds of users simultaneously.

Going forward I want to improve my ability to manage Active Directory through PowerShell and the command line rather than relying solely on the graphical interface. I also want to learn more about creating shared folders with group based permissions so that only authorized security groups can access specific resources — connecting Active Directory group membership directly to file system access control.

## Screenshots

### Proxmox DC01 VM
![Proxmox DC01](01_proxmox_DC01_vm_created.png)

### Windows Server Setup
![Windows Server Setup](02_windows_server_setup_wizard.png)

### Windows Server Installing
![Installing](03_windows_installing.png)

### Windows Server Installed
![Installed](04_windows_server_installed.png)

### Server Manager
![Server Manager](05_windows_server_manager.png)

### VirtIO Drivers Installed
![VirtIO Drivers](06_virtio_drivers_installed.png)

### Static IP Configured
![Static IP](07_static_ip_configured.png)

### DC01 Server Properties
![Server Properties](08_DC01_server_properties.png)

### AD DS Installed
![AD DS Installed](09_AD_DS_installed.png)

### AD DS Review Options
![Review Options](10_AD_DS_review_options.png)

### Prerequisites Passed
![Prerequisites](11_AD_DS_prerequisites_passed.png)

### Domain Controller Login
![DC Login](12_domain_controller_login.png)

### Domain Controller Server Manager
![DC Server Manager](13_domain_controller_server_manager.png)

### OU Structure
![OU Structure](14_AD_OU_structure.png)

### Users Created
![Users](15_AD_users_created.png)

### Group Members
![Group Members](16_AD_group_members.png)

### User Management Options
![User Management](17_AD_user_management_options.png)

### Password Policy
![Password Policy](18_password_policy_configured.png)

### Account Lockout Policy
![Account Lockout](19_account_lockout_configured.png)

### USB Policy
![USB Policy](20_USB_policy_configured.png)

### Security Options
![Security Options](21_security_options_configured.png)

### Windows 10 Installing
![Windows 10 Installing](22_windows10_installing.png)

### Windows 10 Desktop
![Windows 10 Desktop](23_windows10_desktop.png)

### Network Connectivity Verified
![Network Connectivity](24_network_connectivity_verified.png)

### Domain Join Success
![Domain Join](25_domain_join_success.png)

### Group Policy Login Banner
![Login Banner](26_group_policy_login_banner.png)

### Domain User Login Success
![Domain Login](27_domain_user_login_success.png)

### WKSTN01 Domain Confirmed
![WKSTN01 Domain](28_WKSTN01_domain_confirmed.png)

### WKSTN01 in Active Directory
![WKSTN01 in AD](29_WKSTN01_in_AD.png)
