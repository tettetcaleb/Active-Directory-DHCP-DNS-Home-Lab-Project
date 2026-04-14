Active Directory, DHCP & DNS – Home Lab Documentation

What is Active Directory?
Active Directory (AD) is a directory service developed by Microsoft that acts as a centralized system for managing users, computers, and resources within a network. It allows administrators to control access, apply security policies, and manage all devices from one place.

Lab Setup
To simulate a real enterprise environment, I created two virtual machines. One running Windows Server 2022 which acts as the Domain Controller, and one running Windows 10 which acts as the client machine.
Installing Remote Server Administration Tools (RSAT)
Before managing Active Directory from the client machine, I installed the Remote Server Administration Tools on the Windows 10 VM. This gave me access to the Active Directory Users and Computers console.


Installing Active Directory Domain Services
On the Windows Server VM, I installed the Active Directory Domain Services role through Server Manager and promoted the server to a Domain Controller, creating a new forest with the domain mylab.local.

Connecting the Two VMs
Initially the Windows 10 machine could not reach the domain controller due to a network configuration issue. I resolved this by setting both VMs to the same Internal Network in VirtualBox, assigning a static IP address to the Windows Server, and updating the DNS server address on the Windows 10 machine to point to the server's IP address.

Joining the Domain
On the Windows 10 VM I navigated to System Advanced Settings and changed the domain to mylab.local, successfully joining the client machine to the domain.
Creating a User Account
On the Windows Server I opened Active Directory Users and Computers and created a new user named Caleb, granting it administrative privileges on the domain.

Remote Computer Management
After joining the domain I was able to access Computer Management on the Windows 10 machine remotely from the server, demonstrating centralized management capabilities.

Next Steps
I plan to configure DHCP to automatically assign IP addresses to domain joined machines and explore Group Policy Objects to push settings across the domain.
