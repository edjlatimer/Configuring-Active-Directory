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
- Log into the [Azure Portal](https://portal.azure.com/).  
- Create a **Resource Group**:  
  - Search for *Resource Groups* → Click **Create** → Enter name (e.g., `Active-Directory-Lab`).  
- Create a **Virtual Network (VNet)**:  
  - Search for *Virtual Networks* → Click **Create** → Choose the `Active-Directory-Lab` resource group.  
  - Name it `Active-Directory-VNet`.  
  - The **Subnet** could be left at the default setting.

![AD resource group](https://github.com/user-attachments/assets/78a163db-d54f-4e27-999b-6a714a53bce9)

![AD VNet ](https://github.com/user-attachments/assets/e4b34ace-6637-443d-beaa-0c1cb6b1d567)

---

### 🔹 2. Deploy the Domain Controller (DC-1)    
- Create a Virtual Machine:  
  - Virtual Machine Name: `DC-1`
  - Resource Group: `Active-Directory-Lab`
  - Virtual Network: `Active-Directory-VNet`
  - Region: `(Same Region as Resource Group & VNet)`
  - Image: `Windows Server 2022 Datacenter: Azure Edition - Gen2`
  - Size: `Standard D2s v3 (2 vcpus, 8 GiB memory)`
  - Username: `labuser`  
  - Password: `*********`  
![AD dc-1](https://github.com/user-attachments/assets/355e91ab-9082-445b-8e8c-cc47ba01ef2b)
![AD dc-1 deploy complete](https://github.com/user-attachments/assets/df43eb80-bb2d-4e4c-bfc5-d3a982990e30)

---

- Configure **Dc-1 Private IP Address From Dynamic To Static Private IP**:  
  - Go to `DC-1` → Networking → NIC settings → Change IP from *Dynamic* to *Static*.  

![AD dc-1 to static ip](https://github.com/user-attachments/assets/6b0eb6fb-bcde-42f1-bbbb-d1d892ee5061)
![AD dc-1 to static 2](https://github.com/user-attachments/assets/063732bf-cd69-4ad7-a767-d8b69a428ffd)

---

- Connect to `DC-1` VM with **Remote Desktop**.  
- Inside the `DC-1` VM: You should be brought to a `Server Manager Dashbroad`
  (That'll confirm you have successfully setup your DC-1 Server correctly)
- Now, Open **Windows Defender Firewall** from Start menu 
  - Within **Windows Defender Firewall Properties** ensure that **Firewall state** for:
    - Domain Profile
    - Private Profile
    - Public Profile

   are ALL set to **Off** (temporarily to simplify connectivity testing)  

![AD server manager dash](https://github.com/user-attachments/assets/864822c1-151e-4f44-9397-327c32f2e320)
![AD windows firewall OFF](https://github.com/user-attachments/assets/55d76797-b1ea-4599-9252-54832843cd9c)


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
