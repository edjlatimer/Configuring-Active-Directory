# 🖥️ Preparing AD Infrastructure Using Azure 

<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Active Directory Logo" width="300"/>
</p>  

## 📌 Overview  
This project demonstrates how to prepare the proper infrastructure to successfully deploy and configure an **Active Directory environment** using Microsoft Azure.  

The setup includes:  
- A **Domain Controller** (Windows Server 2022)  
- A **Client Machine** (Windows 10)  
- Proper **network, DNS, and connectivity settings** to enable AD functionality  

---

## 🛠️ Technologies Used  
- **Microsoft Azure** (Resource Groups, Virtual Networks, Virtual Machines)  
- **Windows Server 2022** (Domain Controller)  
- **Windows 10 (21H2)** (Client Machine)  
- **Active Directory Domain Services (AD DS)**  
- **PowerShell** & **Remote Desktop**  

---

## ⚙️ Step-by-Step Implementation  

### 🔹 1. Create Resource Group & Virtual Network  
**Execution:**  
- Log into the [Azure Portal](https://portal.azure.com/).  
- Create a **Resource Group**:  
  - Search for *Resource Groups* → Click **Create** → Enter name (e.g., `AD-Lab`).  
- Create a **Virtual Network (VNet)**:  
  - Search for *Virtual Networks* → Click **Create** → Choose the `AD-Lab` resource group.  
  - Name it `AD-VNet`.  
  - Create a **subnet** (e.g., `AD-Subnet`).  

📸 Screenshot Example:  
![Resource Group & VNet](https://via.placeholder.com/600x300?text=Azure+Resource+Group+%26+VNet)  

---

### 🔹 2. Deploy the Domain Controller (DC-1)  
**Execution:**  
- Create a **Windows Server 2022 VM**:  
  - VM Name: `DC-1`  
  - Username: `labuser`  
  - Password: `Cyberlab123!`  
  - Attach to `AD-VNet` and `AD-Subnet`.  
- Configure **Static Private IP**:  
  - Go to `DC-1` → Networking → NIC settings → Change IP from *Dynamic* to *Static*.  
- Connect to VM with **Remote Desktop**.  
- Inside the VM:  
  - Open *Windows Defender Firewall*.  
  - Turn it **Off** temporarily (to simplify connectivity testing).  

📸 Screenshot Example:  
![Domain Controller Setup](https://via.placeholder.com/600x300?text=Domain+Controller+Setup)  

---

### 🔹 3. Deploy the Client Machine (Client-1)  
**Execution:**  
- Create a **Windows 10 VM**:  
  - VM Name: `Client-1`  
  - Username: `labuser`  
  - Password: `Cyberlab123!`  
  - Attach to the same `AD-VNet` and `AD-Subnet`.  
- Update **DNS settings**:  
  - Go to `Client-1` → Networking → DNS Servers → Set to **Custom DNS** = `DC-1`’s private IP.  
  - Save and **Restart** the VM from the Azure Portal.  
- Connect to `Client-1` with RDP.  

📸 Screenshot Example:  
![Client VM Setup](https://via.placeholder.com/600x300?text=Client+VM+Setup)  

---

### 🔹 4. Verify Connectivity Between Client and DC  
**Execution:**  
- On `Client-1`, open **Command Prompt**:  
  - Run:  
    ```bash
    ping <DC-1 Private IP>
    ```  
  - Confirm replies are received.  
- On `Client-1`, open **PowerShell**:  
  - Run:  
    ```powershell
    ipconfig /all
    ```  
  - Verify the **DNS Server** is set to `DC-1`’s private IP.  

📸 Screenshot Example:  
![PowerShell Verification](https://via.placeholder.com/600x300?text=PowerShell+Verification)  

---

## 🎯 Outcome / Learnings  
By completing this project, I:  
- Deploy a fully functional **Active Directory environment** inside Azure.  
- Configure **static IP addressing** and custom DNS settings.  
- Validate communication between a **Domain Controller and client machine**.  
- Strengthen skills in **networking, VM management, and directory services**.  

---
