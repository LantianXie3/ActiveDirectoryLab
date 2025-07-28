<h1>🏠 Active Directory Home Lab Setup (VirtualBox + PowerShell)</h1>


<img width="100%" height="10%" alt="image" src="https://github.com/user-attachments/assets/fbae5887-3081-463e-a451-66578cdb117e" />

<h2>📝 Description</h2>

This project demonstrates how to build a simple home lab using **Oracle VirtualBox** and **Windows Server 2019** to create a fully functioning **Active Directory Domain Services (AD DS)** environment. It also shows how to use a **PowerShell script** to automatically create users inside Active Directory.

The purpose is to simulate a real-world IT environment for training, home lab practice, or resume-building. By the end, you'll have a working domain controller and a script that can populate it with user accounts — a great foundation for future experimentation.

---

<h2>🛠️ Technologies & Tools Used</h2>

- <b>PowerShell</b> – For scripting user creation  
- <b>Oracle VirtualBox</b> – Hypervisor for virtual machines  
- <b>Windows Server 2019</b> – Domain controller setup  
- <b>Active Directory Domain Services (AD DS)</b>

---

<h2>🖥️ Environments Used</h2>

- Windows Server 2019 (inside VM)  
- Windows 10 (host OS or client VM, optional)  
- Oracle VirtualBox

---

<h2>📸 Walkthrough (with Screenshots)</h2>

<p align="center">
<b>VirtualBox VM Configuration</b><br />
<img width="1187" height="584" alt="image" src="https://github.com/user-attachments/assets/64cfafa9-09a2-461a-85c9-4055cebfde37" />

### Create Virtual Machines in VirtualBox

- Open Oracle VirtualBox and click "New."
- Name the first machine as "DC" and select Microsoft Windows as the type and Windows (64-bit) as the version.
- Assign at least 2 GB of RAM and create a new virtual hard disk of at least 50 GB.
- Repeat the process for our Windows 10 machine and name it "CLIENT."
</p>

### Installing the Operating Systems and Initial Setup

I initiated the installations by attaching the ISO files to their respective VMs. Windows Server 2019 required additional attention during setup to ensure proper administrative access. Here are the specifics:

- **Install Windows Server 2019:** Follow the on-screen instructions until you reach the account setup stage.
  
<img width="952" height="699" alt="image" src="https://github.com/user-attachments/assets/2e93408b-531d-4c1b-927e-752804f87232" />

  
- **Create an Admin Account:** As part of the installation, you'll be prompted to create an administrator account. Here, I provided a username and a robust and unique password for enhanced security. This account will have elevated permissions, so it's crucial to protect it adequately.

<img width="984" height="751" alt="image" src="https://github.com/user-attachments/assets/2cc6a7bd-04ad-4ccc-b23e-576ab16526e3" />

<img width="1003" height="756" alt="image" src="https://github.com/user-attachments/assets/e489b840-4086-4701-90ca-3b7ab14f8183" />


### Laying Down the Operating Systems and Configuring NICs

I initiated the installation process after attaching the ISO files to their respective VMs. The on-screen instructions are generally straightforward, leading to a successful installation of both operating systems.

In addition, for our Domain Controller, it's imperative to configure dual Network Interface Cards (NICs) for effective network management. Here's how I set them up

- **NIC for Open Internet:** The first NIC is configured to use Network Address Translation (NAT), enabling our Domain Controller to connect to the open Internet.
  
- **NIC for Internal Network:** The second NIC is allocated exclusively to our internal virtual network, ensuring isolated communication within our simulated environment.

This dual NIC setup equips the Domain Controller with the network versatility needed for real-world applications, aligning perfectly with our lab's objectives. 

Doing so has established a secure yet flexible network topology indispensable for our Active Directory Lab.




## Integrating Active Directory Domain Services

- Upon booting up Windows Server, I ventured into the Server Manager, and from there, I meticulously added the "Active Directory Domain Services" role.

<img width="1234" height="445" alt="image" src="https://github.com/user-attachments/assets/c2630402-2273-4ce0-94a1-8e4209989390" />



## Creating a Domain Admin Account

- Open Active Directory Users and Computers.
- Right-click and select New > User.
- Provide details for your new Domain Admin account and set a strong password.

## Set Up DHCP Server

- In Server Manager, I added the "DHCP Server" role to enable our client machine to automatically receive an IP address through dynamic allocation.
- 
## Structuring the Organizational Unit

- Open Active Directory Users and Computers.
- Right-click on your domain, go to New > Organizational Unit, and create an OU.

## Configure Routing and Remote Access

- Go to Server Manager and add the "Routing" role.
- Configure Routing and Remote Access to set up an internal network.

## Configure NIC for Internet Access

- Open Network and Sharing Center and go to "Change adapter settings."
- Configure the NIC to connect to the outside internet.

## Add Users via PowerShell

In a typical organizational environment, it's common to manage hundreds or even thousands of users. Manually creating each account would be highly inefficient. To streamline this process, I utilized a PowerShell script for bulk user creation. Let’s break down the BulkAddUserScript.ps1 script to understand how it works and what it does.

### Define Variables:

    # ----- Edit these Variables for your own Use Case ----- #
    $PASSWORD_FOR_USERS   = "Password1"
    $USER_FIRST_LAST_LIST = Get-Content .\names.txt
    # ------------------------------------------------------ #

Here, two variables are being defined:

- ```$PASSWORD_FOR_USERS = "Password1":``` This variable defines a default password—set to "Password1"—for every user account. While using a shared password isn’t ideal in production environments due to security concerns, it’s acceptable for this controlled lab setting.
- ```$USER_FIRST_LAST_LIST = Get-Content .\names.txt:``` This command reads from the file names.txt, which contains a list of names formatted as "FirstName LastName", one per line. These names are used to generate user accounts automatically.
  
### Convert Plain Text Password:

      $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

- PowerShell user management commands typically require passwords to be in a secure string format for security purposes. In this step, the plain text password stored in the ```$PASSWORD_FOR_USERS``` variable is converted into a secure string and saved in the $password variable.
  
### Create Organizational Unit (OU):

    New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

- In this step, an Organizational Unit (OU) named `_USERS` is created. The `-ProtectedFromAccidentalDeletion $false` parameter means the OU can be accidentally deleted, which is acceptable for a lab setting. However, in a production environment, it's recommended to enable protection.


### Loop Through Names and Create Users:

    foreach ($n in $USER_FIRST_LAST_LIST) {
    ...
    }

- Each name in the ($USER_FIRST_LAST_LIST) is processed through the following set of actions.
  
### Extract the first and last names:

    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()

- The `.Split(" ")` method divides the full name into first and last names using the space as a separator. After that, the `.ToLower()` method converts both names to lowercase.


### Construct the username:
    
    $username = "$($first.Substring(0,1))$($last)".ToLower()
- This line builds the username by combining the first letter of the first name with the entire last name, all in lowercase. For instance, if the name is `"John Doe"`, the resulting username would be `"jdoe"`.


### Display progress in the console:

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
- This line outputs a message to the console indicating the user that is being created.
- 
<img width="1496" height="657" alt="image" src="https://github.com/user-attachments/assets/4eb106cb-f99b-4669-ad14-2e248421c0a5" />



### Create the new user:


    New-AdUser ...

- The `New-AdUser` cmdlet is used to create a new user in Active Directory. The script sets several parameters like given name, surname, display name, and more, placing the user in the `_USERS OU` created earlier. The `-PasswordNeverExpires $true` parameter prevents the user's password from expiring, which is acceptable for a lab setup, but in a live production environment, you typically want to enforce password expiration.


### Why are we doing this?

This script is primarily designed to highlight the automation potential in managing Active Directory, particularly when handling numerous user accounts. Instead of manually adding each user—a process that can be slow and prone to mistakes—this script automates the workflow, improving accuracy and saving time. It demonstrates how PowerShell can effectively simplify administration tasks within Windows environments, providing a practical example of its usefulness in everyday IT operations.


### Client Interaction and User Experience

After setting up our Windows 10 VM, I configured it to connect to the server environment. Then, I logged out and logged back in to simulate the experience of a user created in the batch process.

<img width="999" height="744" alt="image" src="https://github.com/user-attachments/assets/66b22257-7758-4880-ac5f-3607998fb40b" />



## Final Thoughts and Conclusions

### Achieved Objectives

This lab successfully walked through the process of setting up and configuring a Windows-based Active Directory environment. From the initial installation and domain setup to configuring network services and implementing automation, each step reflected real-world IT practices. The use of a PowerShell script for bulk user creation showcased the scalability and efficiency possible through automation.

### Learning Outcomes

By engaging in this hands-on lab, we gained practical experience with key components of Active Directory and Windows networking. Topics like DHCP configuration, user and OU management, and PowerShell scripting were made more approachable through direct application. The ability to automate administrative tasks stood out as a particularly valuable takeaway.

### Real-World Applicability

The knowledge and skills developed in this lab are directly applicable to professional IT environments. Active Directory plays a central role in organizational infrastructure, and understanding how to configure and manage it is essential for IT administrators. The focus on automation further prepares us to handle large-scale deployments and reduce manual overhead.

### Future Exploration

While this lab covered core aspects of Active Directory, there remains much to explore. Future learning could include:

    Group Policy Objects (GPO)

    Advanced security settings

    Multi-site replication

    Integration with cloud-based services like Azure Active Directory

Diving into these topics will help expand and deepen your understanding of enterprise-level directory services.

### Closing Remarks

In conclusion, this lab served as a practical introduction to Windows-based networking and Active Directory administration. It provided not only the technical knowledge needed to set up a functioning domain environment, but also a strategic perspective on its role in enterprise IT.

Though Active Directory can seem complex, this lab demonstrated that a structured, hands-on approach can build both competence and confidence. For those entering the IT field or advancing within it, mastering Active Directory is a crucial step — and this lab provides a solid foundation to build upon.
