Active Directory, DHCP & DNS – Home Lab Documentation

What is Active Directory?

Active Directory (AD) is a directory service developed by Microsoft that acts as a centralized system for managing users, computers, and resources within a network. It allows administrators to control access, apply security policies, and manage all devices from one place.

Lab Setup

To simulate a real enterprise environment, I created two virtual machines. 
-One running Windows Server 2022 which acts as the Domain Controller 
-One running Windows 10 which acts as the client machine.
![Screenshot](screenshots/Screenshot-2026-04-14-053255.png)

Installing Remote Server Administration Tools (RSAT)
Before managing Active Directory from the client machine, I installed the Remote Server Administration Tools on the Windows 10 VM. This gave me access to the Active Directory Users and Computers console.


Installing Active Directory Domain Services

On the Windows Server VM, I installed the Active Directory Domain Services role through Server Manager and promoted the server to a Domain Controller, creating a new forest with the domain mylab.local.

Connecting the Two VMs

Initially the Windows 10 machine could not reach the domain controller due to a network configuration issue. I resolved this by:
-Creating a Nat network in VirtualBox    ![Screenshot](screenshots/Screenshot-2026-04-14-053512.png) 
-Setting both VMs to the same Nat Network in VirtualBox  ![Screenshot](screenshots/Screenshot-2026-04-14-053602.png)  
-Updating the DNS server address on the Windows 10 machine to point to the server's IP address.

Joining the Domain
On the Windows 10 VM I navigated to System Advanced Settings and changed the domain to mylab.local, successfully joining the client machine to the domain.![Screenshot](screenshots/Screenshot-2026-04-14-053757.png)

Creating a User Account

On the Windows Server I opened Active Directory Users and Computers and created a new user named Caleb, granting it administrative privileges on the domain.

Remote Computer Management

After joining the domain I was able to access Computer Management on the Windows 10 machine remotely from the server, demonstrating centralized management capabilities. ![Screenshot](screenshots/Screenshot-2026-04-14-054121.png)

Group policy object

In my domain computer i added a new group called HR which has certain restrictions and ability to intall new software. This is very usefull and allows me to group people working togerther depending on what restrictions and accesses they need.  ![Screenshot](screenshots/Screenshot-2026-04-14-053048.png)

DYNAMIC HOST CONFIGURATION PROTOCOL

First was to set us DHCP on my windows server
![Screenshot](screenshots/Screenshot-2026-04-14-073517.png)

SCOPE - IP address range that is used for a DHCP server

To create a new scope i had to:
- install DHCP on my windows server
- create the new scope
  ![Screenshot](screenshots/Screenshot-2026-04-15-025535.png)


Address Pool – The range of IP addresses available to be leased out to clients (e.g., 10.0.2.0 range).

Address Leases – Shows currently active leases; which clients have been assigned an IP, their hostnames, and lease expiration times.

Reservations – Lets you permanently assign a specific IP to a device based on its MAC address, so it always gets the same IP.

Next is to assigh an IP address to a specific mac-address through DHCP
For this example i uesd a random mac address from the internet
 ![Screenshot](screenshots/Screenshot-2026-04-14-074103.png)



DNS (Domain Name System)

DNS is essentially the phonebook of the internet — it translates human-readable domain names (like google.com) into IP addresses (like 142.250.80.46) that computers use to communicate.

How it Works

You type google.com in your browser
Your computer asks a DNS server "what's the IP for google.com?"
The DNS server responds with the IP address
Your computer connects to that IP


Key Uses

Name Resolution – Converts hostnames to IPs and vice versa

Website Browsing – Every website visit starts with a DNS lookup

Email Routing – MX records tell mail servers where to deliver email

Internal Networks – Resolves local hostnames (e.g., mylab.local) within a private network

Load Balancing – Can point one domain to multiple IPs to distribute traffic

Security (DNSSEC) – Validates DNS responses to prevent spoofing attacks

Assigning a Static IP to My Domain Through DNS

In this step, I assigned a static IP address to my Windows Server and configured it to act as the DNS server for my mylab.local domain. This ensures that all clients on the network can always find the server at a known, fixed address.
 ![Screenshot](screenshots/Screenshot-2026-04-15-033620.png)


DNS A Record

 An A record (Address record) is the most basic DNS record — it maps a hostname to an IPv4 address.

 How I created an a record for my printer :

-Open DNS Manager on my Windows Server

- Forward Lookup Zones

-Click on mylab.local

-Right-click in the right pane → New Host (A or AAAA)
-Fill in:
-Name: NetworkPrinter
-IP Address: e.g. 10.0.2.20
-Check "Create associated PTR record" 
- Add Host


# 🖥️ Active Directory, DHCP & DNS — Home Lab Documentation

## Overview
Built a home lab simulating a real enterprise environment using Oracle VirtualBox. Configured a Windows Server 2022 Domain Controller alongside a Windows 10 client machine, then deployed and tested Active Directory, DHCP, and DNS services.

---

## 🏗️ Lab Setup

Two virtual machines were created:
- **Windows Server 2022** — Acts as the Domain Controller
- **Windows 10** — Acts as the client machine

 ![Screenshot](screenshots/Screenshot-2026-04-14-053255.png)

---

## 🔧 Configuration Steps

### 1. Creating a NAT Network in VirtualBox
Created a shared NAT Network (`NatNetwork`, `10.0.2.0/24`) so both VMs could communicate with each other.

 ![Screenshot](screenshots/Screenshot-2026-04-14-053512.png)

---

### 2. Connecting Both VMs to the Same Network
Set both the Windows Server and Windows 10 VMs to use the same `NatNetwork` adapter, then updated the DNS server on the Windows 10 machine to point to the server's IP.

 ![Screenshot](screenshots/Screenshot-2026-04-14-053602.png)

---

### 3. Joining the Client to the Domain
On the Windows 10 VM, navigated to **System > Advanced Settings** and joined the `mylab.local` domain. The machine was renamed `WORK-0001`.

 ![Screenshot](screenshots/Screenshot-2026-04-14-053757.png)

---

### 4. Remote Computer Management
After joining the domain, accessed **Computer Management** on the Windows 10 machine remotely from the server — demonstrating centralized administration.

 ![Screenshot](screenshots/Screenshot-2026-04-14-054121.png)

---

### 5. Group Policy — HR Group
Created an HR security group in Active Directory with specific restrictions and software permissions. Group Policy Objects allow administrators to apply different rules to different departments.

 ![Screenshot](screenshots/Screenshot-2026-04-14-053048.png)

---

### 6. Installing DHCP on Windows Server
Installed the **DHCP Server** role via Server Manager's Add Roles and Features Wizard.

 ![Screenshot](screenshots/Screenshot-2026-04-14-073517.png)

---

### 7. Creating a DHCP Scope
Created a new scope with an IP address range of `10.0.2.100` – `10.0.2.200` to dynamically assign addresses to clients on the network.

 ![Screenshot](screenshots/Screenshot-2026-04-15-025535.png)

---

### 8. DHCP Reservation — MAC Address Binding
Created a DHCP reservation to permanently assign IP `10.0.2.20` to a device (simulated printer) using its MAC address, ensuring it always receives the same IP.

 ![Screenshot](screenshots/Screenshot-2026-04-14-074103.png)

---

### 9. Assigning a Static IP to the Domain Controller
Configured the Windows Server's NIC with a static IP (`10.0.2.15`) and set it as the DNS server for the `mylab.local` domain, ensuring reliable name resolution for all clients.

 ![Screenshot](screenshots/Screenshot-2026-04-15-033620.png)

---

### 10. Creating a DNS A Record
In DNS Manager, created an A record mapping `NetworkPrinter.mylab.local` → `10.0.2.20`, with an associated PTR record for reverse lookup.

 ![Screenshot](screenshots/Screenshot-2026-04-15-034506.png)

---

## 🧠 Configuration Summary

| Service | Task | Detail |
|---------|------|--------|
| Active Directory | Domain setup | `mylab.local` forest, DC on Windows Server 2022 |
| Active Directory | Client join | `WORK-0001` joined to `mylab.local` |
| Active Directory | Group Policy | HR group with custom restrictions |
| DHCP | Scope | `10.0.2.100` – `10.0.2.200` |
| DHCP | Reservation | Printer bound to `10.0.2.20` via MAC |
| DNS | Static IP | Server at `10.0.2.15` |
| DNS | A Record | `NetworkPrinter.mylab.local` → `10.0.2.20` |

---

## 📋 Skills Demonstrated
- Active Directory Domain Services setup and domain join
- Group Policy Object (GPO) creation and group management
- DHCP scope configuration and MAC-based reservations
- DNS A record and PTR record creation
- VirtualBox NAT network configuration for VM-to-VM communication
- Remote server administration (RSAT)

---

## 🛠️ Tools & Technologies

![Windows Server](https://img.shields.io/badge/Windows_Server_2022-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Windows_10-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active_Directory-003087?style=for-the-badge&logo=microsoft&logoColor=white)

 ![Screenshot](screenshots/Screenshot-2026-04-15-034506.png)
