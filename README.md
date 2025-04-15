<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell
- Notepad/Notes App (Needed for saving usernames and passwords)

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Prepare Active Directory Infrastructure within Azure</h2>

- Create Resource Group, Virtual Network and Subnet within Azure
- Create and configure Domain Controller(DC) virtual machine within the resource group
- Set Domain Controller VM NIC Private IP Address to static
- Logon to Domain Controller virtual machine and disable the Windows Firewall
- Create and configure a Client virtual machine
- Set Client's VM DNS settings to Domain Controller(DC) VM's private IP Address, then restart VM from Azure
- Log into Client VM and attempt to ping Domain Controller's private IP Address, them ping (run "ipconfig /all") within Powershell as an admin
- Finally, expected result should be the IP Address of the Domain Controller VM

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/qTzUliP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h3>Active Directory Installation/Setting up new forest for domain controller</h3>
<p>
<ol>
  <li>Log in to the Domain Controller (DC) virtual machine.</li>
  <li>Open the <strong>Start Menu</strong>, search for <strong>Server Manager</strong>, and launch the application.</li>
  <li>Once Server Manager is loaded, close any pop-up notifications.</li>
  <li>Select <strong>"Add roles and features"</strong> from the dashboard.</li>
  <li>Click <strong>Next</strong> through the initial prompts until you reach the <strong>Server Roles</strong> section.</li>
  <li>In the <strong>Server Roles</strong> section, check the box for <strong>Active Directory Domain Services</strong>.</li>
  <li>When prompted, select <strong>"Add Features"</strong>, then continue clicking <strong>Next</strong> until you reach the <strong>Confirmation</strong> screen.</li>
  <li>On the Confirmation screen, check the box labeled <strong>"Restart the destination server automatically if required"</strong> and click <strong>Install</strong>.</li>
  <li>Once the installation is complete, click the <strong>flag icon</strong> in the top-right corner of the Server Manager and select <strong>"Promote this server to a domain controller"</strong>.</li>
  <li>Under the <strong>Deployment Configuration</strong> tab, choose <strong>"Add a new forest"</strong>.</li>
  <li>In the <strong>Root domain name</strong> field, enter a domain name.
    <ol>
      <li>For this lab, use: <code>mydomain.com</code></li>
    </ol>
  </li>
  <li>Click <strong>Next</strong>, then set a simple <strong>Directory Services Restore Mode (DSRM) password</strong>, and click <strong>Next</strong>.</li>
  <li>On the DNS Options screen, uncheck the box labeled <strong>"Create DNS delegation"</strong>, then click <strong>Next</strong> through the remaining steps.</li>
  <li>When you reach the <strong>Prerequisites Check</strong> screen, click <strong>Install</strong>.</li>
  <li>The virtual machine will automatically restart upon completion.</li>
  <li>After the restart, log back into the VM.
    <ol>
      <li>When prompted for the username, use the following format: <code>[domain_name]\[username]</code></li>
      <li>Example: <code>mydomain.com\labuser</code></li>
    </ol>
  </li>
</ol>
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now will be creating a domain admin user within the domain. Within DC vm click the start menu search/find Active Directory Users and Computers and open up the application. On the left panel hover a right click mydomain.com and then hover over New and the click Organizational Unit(OU). Give the folder a name, this will contain all the user accounts you have access to as admin. For this lab we will name this "_EMPLOYEES". Wihtin this folder will contain access to all employee accounts.  If you chose to change the name of the folder differently make sure to change the script according to the name of the folder you've given  later on in this lab. Next create another Organizational Unit(OU). For this lab we will name this folder "_ADMINS". Within this dolder will contain access to all admins accounts. Now will we create a new employee and add/make them an admin for this domain. For this lab we create an employee name Jane Doe with her own username and password. To do right click the _ADMINS folder and hover over new and select Users. After that fill the necessary information, such as name, username, then click next and then enter a password. For this lab the username for jane doe will be jane_admin
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
