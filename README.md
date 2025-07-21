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
<b>1. VirtualBox VM Configuration</b><br />
<img src="https://your-image-link.com/vm-setup.png" width="80%" alt="VM Configuration"><br />
Setting up a new VirtualBox VM with at least 2GB RAM, bridged or internal network adapter, and a 64GB virtual hard drive. Windows Server 2019 ISO is mounted for installation.
</p>

<p align="center">
<b>2. Installing Windows Server</b><br />
<img src="https://your-image-link.com/install-server.png" width="80%" alt="Installing Server"><br />
Begin the installation process for Windows Server 2019. After installation, set a strong admin password and install updates.
</p>

<p align="center">
<b>3. Adding Active Directory Domain Services</b><br />
<img src="https://your-image-link.com/install-adds.png" width="80%" alt="Install ADDS"><br />
Using Server Manager, install the AD DS role to allow this machine to become a Domain Controller. Reboot when prompted.
</p>

<p align="center">
<b>4. Promote Server to Domain Controller</b><br />
<img src="https://your-image-link.com/promote-dc.png" width="80%" alt="Promote to Domain Controller"><br />
Configure a new forest and choose a root domain name (e.g., <i>home.lab</i>). The server will then promote itself to a DC and reboot.
</p>

<p align="center">
<b>5. Confirm Active Directory is Installed</b><br />
<img src="https://your-image-link.com/ad-confirm.png" width="80%" alt="AD Confirmed"><br />
Launch "Active Directory Users and Computers" to confirm that the domain is set up correctly. You’ll see the domain listed in the left pane.
</p>

<p align="center">
<b>6. PowerShell Script to Create Users</b><br />
<img src="https://i.imgur.com/uKZFWgV.png" width="80%" alt="Users Created"><br />
This screenshot shows a PowerShell script used to create multiple users in Active Directory from a CSV file. The script loops through each row in the CSV and uses `New-ADUser` to generate the user accounts automatically. This simulates a real-world IT task like onboarding new employees and helps automate repetitive administrative processes.
</p>


