# AD-Permissions-Lab
A hands-on Active Directory lab configuring Security Groups, network file shares, and NTFS permissions to restrict user data access.
# Active Directory User File Access and Permissions Lab

## Project Overview
This lab demonstrates the implementation of secure file sharing and access control within a Windows Server domain environment using Active Directory Domain Services (AD DS). The goal was to enforce the principle of least privilege by combining Network Share Permissions with NTFS Permissions to restrict data access to authorized security groups only.

## Environment & Architecture
* **Domain Controller:** Windows Server (Domain: `JUMBO.local`)
* **Client Machine:** Windows 10/11 Enterprise joined to the domain
* **Hypervisor:** Oracle VirtualBox

---

## Step-by-Step Implementation

### 1. Active Directory Object Creation
* Navigated to **Active Directory Users and Computers (ADUC)**.
* Created a dedicated Organizational Unit (OU) to manage department assets.
* Provisioned domain user accounts to simulate different staff roles.
* Created a **Security Group** and assigned the authorized user as a member.

![AD Users and Groups](/01-ou-and-users.png)
![Security Group Members](/02-security-group.png)

### 2. Network Share Configuration
* Created the target directory on the server file system.
* Enabled **Advanced Sharing** for the folder.
* Configured the Share-level permissions to grant **Full Control** to `Domain Users` to ensure network accessibility, leaving specific restrictions to the NTFS layer.

![Folder Sharing Settings](/03-folder-sharing.png)

### 3. Enforcing NTFS Permissions (Access Control)
* Opened the folder's **Security** properties and advanced settings.
* **Disabled Inheritance** to remove default permissions inherited from the parent drive, converting them to explicit permissions.
* Removed standard user permissions and added the newly created **Security Group**.
* Assigned explicit **Read/Write/Modify** permissions to the group while blocking all other non-administrative domain accounts.

![NTFS Explicit Permissions](/04-ntfs-permissions.png)

---

## Verification & Testing

To validate the configuration, I logged into the client VM domain member under both user profiles to test the network path access (`\\<Server-IP-or-Name>`).

* **Authorized User Test:** Successfully authenticated, mapped the network folder, and managed files.
  
  ![Access Granted Verification](/05-access-allowed.png)

* **Unauthorized User Test:** Correctly blocked by the system, throwing an explicit "Windows cannot access... Access is denied" security prompt.

  ![Access Denied Verification](/06-access-denied.png)
