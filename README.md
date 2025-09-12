# 🛠️ Active Directory Domain Services on Azure

![Azure](https://img.shields.io/badge/Azure-Cloud-blue?logo=microsoft-azure)  
![WindowsServer](https://img.shields.io/badge/Windows-Server%202022-blue?logo=windows)  
![ADDS](https://img.shields.io/badge/Active%20Directory-Domain%20Services-green)  

---

## 📌 Project Overview
This project demonstrates how to deploy and configure **Active Directory Domain Services (AD DS)** on a **Windows Server 2022 VM** in Microsoft Azure, along with a **Windows 10 client VM** joined to the domain.

Key tasks include:
- Provisioning Azure resources (VMs, networking)  
- Installing and configuring AD DS  
- Creating Organizational Units (OUs) and domain accounts  
- Joining a client to the domain  
- Configuring Remote Desktop for domain users  
- Automating user creation with PowerShell  

**VM Specifications:**
- **Domain Controller (DC-1):** Windows Server 2022  
- **Client (Client-1):** Windows 10  
- **Username:** labuser  
- **Password:** Cyberlab123!  

---

## 📑 Table of Contents
1. [Provision Domain Controller](#1-provision-domain-controller)  
2. [Configure Client-1](#2-configure-client-1)  
3. [Install Active Directory Domain Services](#3-install-active-directory-domain-services)  
4. [Create OUs and Domain Admin](#4-create-ous-and-domain-admin)  
5. [Join Client-1 to Domain](#5-join-client-1-to-domain)  
6. [Enable RDP for Domain Users](#6-enable-rdp-for-domain-users)  
7. [Automate User Creation with PowerShell](#7-automate-user-creation-with-powershell)  
8. [Lab Completion](#8-lab-completion)  

---

## 🚀 Lab Steps

### 1. Provision Domain Controller
- Create a **Resource Group** in Azure  
- Create a **Virtual Network** + Subnet  
- Deploy **Windows Server 2022 VM** named `DC-1`  
  - Username: `labuser`  
  - Password: `Cyberlab123!`  
- Set **DC-1 NIC Private IP** → Static  
- Disable **Windows Firewall** (testing only)  

---

### 2. Configure Client-1
- Deploy **Windows 10 VM** named `Client-1`  
  - Username: `labuser`  
  - Password: `Cyberlab123!`  
  - Attach to **same VNet/region** as DC-1  
- Set **DNS** on Client-1 → DC-1’s private IP  
- Restart Client-1 from Azure Portal  
- Verify connectivity:  
  - `ping DC-1_Private_IP` → success  
  - `ipconfig /all` → DNS = DC-1’s IP  

---

### 3. Install Active Directory Domain Services
- Log into DC-1  
- Install **Active Directory Domain Services**  
- Promote to Domain Controller → New forest: `mydomain.com`  
- Restart and log in as: `mydomain.com\labuser`  

---

### 4. Create OUs and Domain Admin
In **Active Directory Users and Computers (ADUC):**
- Create OUs:  
  - `_EMPLOYEES`  
  - `_ADMINS`  
- Create **Admin Account**:  
  - Name: *Jane Doe*  
  - Username: `jane_admin`  
  - Password: `Cyberlab123!`  
  - Add to **Domain Admins** group  
- Log out → log in as: `mydomain.com\jane_admin` (use this account going forward)  

---

### 5. Join Client-1 to Domain
- Log into Client-1 as **local** `labuser`  
- Join Client-1 to `mydomain.com`  
- Restart Client-1  
- On DC-1 → verify Client-1 appears in **ADUC**  
- Create OU `_CLIENTS` and move Client-1 into it  

---

### 6. Enable RDP for Domain Users
- Log into Client-1 as `mydomain.com\jane_admin`  
- Open **System Properties → Remote Desktop**  
- Allow **Domain Users** RDP access  
- Now non-admin domain users can log into Client-1  

---

### 7. Automate User Creation with PowerShell
- Log into DC-1 as `jane_admin`  
- Open **PowerShell ISE (Admin)**  
- Run a bulk user creation script to populate `_EMPLOYEES` OU  
- Verify accounts in ADUC  
- Test login on Client-1 with a newly created account  

**Example PowerShell Script:**
```powershell
# Bulk User Creation Script
Import-Module ActiveDirectory

For ($i=1; $i -le 10; $i++) {
    $UserName = "employee$i"
    $Password = ConvertTo-SecureString "Password123!" -AsPlainText -Force
    New-ADUser -Name $UserName `
               -AccountPassword $Password `
               -Path "OU=_EMPLOYEES,DC=mydomain,DC=com" `
               -Enabled $true
}
