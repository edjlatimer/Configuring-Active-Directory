#  Preparing AD Infrastructure Using Azure 

<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Active Directory Logo" width="300"/>
</p>  

##  Overview  
This project demonstrates how to prepare the proper infrastructure to successfully deploy and configure an **Active Directory environment** using Microsoft Azure.  

The setup includes:  
- A **Domain Controller** (Windows Server 2022)  
- A **Client Machine** (Windows 10)  
- Proper **network, DNS, and connectivity settings** to enable AD functionality  

---

##  Technologies Used  
- **Microsoft Azure** (Resource Groups, Virtual Networks, Virtual Machines)  
- **Windows Server 2022** (Domain Controller)  
- **Windows 10 (21H2)** (Client Machine)  
- **Active Directory Domain Services (AD DS)**  
- **PowerShell** & **Remote Desktop**  

---

##  Step-by-Step Implementation  

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
 
- Create a Virtual Machine:  
  - Virtual Machine Name: `Client-1`
  - Resource Group: `Active-Directory-Lab`
  - Virtual Network: `Active-Directory-VNet`
  - Region: `(Same Region as Resource Group & VNet)`
  - Image: `Windows (Windows 10 Pro)`
  - Size: `Standard D2s v3 (2 vcpus, 8 GiB memory)`
  - Username: `labuser`  
  - Password: `*********`

![AD client-1 VM info](https://github.com/user-attachments/assets/74abae0a-beae-4de7-9aa8-7c39f8a42da8)

---

- Update **DNS settings**:  
  - Go to `Client-1` → Networking → DNS Servers → Set to **Custom DNS** → Put `DC-1`’s private IP (`10.0.0.4` for the sake of this project) 
  - Save and **Restart** the `Client-1` VM within Azure  
- Then connect and log into `Client-1` VM with **Remote Desktop**

**Note:** It'll bring you to a Windows OS Home Screen (Not a **`Server Manager Dashbroad`**) once logged in = Successful Windows 10 deployment

![AD client-1 DNS to dc-1](https://github.com/user-attachments/assets/99ca02b3-7a58-4831-a6ae-b340370b1f7b)

![AD client-1 OS home](https://github.com/user-attachments/assets/20141452-a488-48b0-94bf-45ff9f2a9291)


---

### 🔹 4. Verify Connectivity Between Client and DC Using Powershell

- On `Client-1`, open **Powershell**:  
  - Run **`Ping` DC-1 Private IP `(10.0.0.4)`**
      
  - Confirm replies are received.  

- On `Client-1`, open **PowerShell**:  
  - Run **ipconfig /all**
  - Verify the **DNS Server** is set to `DC-1`’s Private IP 

![AD client-1 ping dc-1](https://github.com/user-attachments/assets/242c6f71-030b-48c6-b2e4-33cbbc3fae17)
![AD client-1 ipconfig :all DNS info](https://github.com/user-attachments/assets/597e33af-e5ab-42e8-b9f8-9c3b02aae08f)

---

##  Outcome / Learnings  
By completing this project, I:  
- Prepared a fully functional **Active Directory Infrastructure** using Azure  
- Configured **static IP addressing** and custom DNS settings.  
- Validated communication between a **Domain Controller and client machine**.  
- Strengthen skills in **networking, VM management, and directory services**.  

---
