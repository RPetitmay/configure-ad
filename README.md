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
<img src="https://i.imgur.com/A8xi5XW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <h3>Create a Domain Admin User Account</h3>
<ol>
  <li>Log in to the Domain Controller (DC) virtual machine.</li>
  <li>Open the <strong>Start Menu</strong>, search for <strong>Active Directory Users and Computers</strong>, and launch the application.</li>
  <li>In the left-hand panel, right-click on the domain name (e.g., <code>mydomain.com</code>), hover over <strong>New</strong>, and select <strong>Organizational Unit (OU)</strong>.</li>
  <li>Name the new Organizational Unit.
    <ol>
      <li>For this lab, name it <strong>_EMPLOYEES</strong>.</li>
      <li>This OU will store all user accounts with standard access privileges.</li>
      <li>If a different name is used, update any associated scripts or references accordingly.</li>
    </ol>
  </li>
  <li>Repeat the process to create another Organizational Unit.
    <ol>
      <li>Name this one <strong>_ADMINS</strong>.</li>
      <li>This OU will store all accounts with administrative privileges.</li>
    </ol>
  </li>
  <li>Create a new user to act as a domain administrator.
    <ol>
      <li>Right-click the <strong>_ADMINS</strong> OU, hover over <strong>New</strong>, and select <strong>User</strong>.</li>
      <li>Enter the user's full name and username.
        <ol>
          <li><strong>Name:</strong> Jane Doe</li>
          <li><strong>Username:</strong> <code>jane_admin</code></li>
        </ol>
      </li>
      <li>Click <strong>Next</strong>, enter a password, and select <strong>Password never expires</strong>.
        <ol>
          <li><em>Note: Selecting "Password never expires" is not recommended in production environments.</em></li>
        </ol>
      </li>
      <li>Click <strong>Finish</strong> to complete the user creation process.</li>
    </ol>
  </li>
  <li>Verify the user was created by checking the <strong>_ADMINS</strong> folder for <code>jane_admin</code>.</li>
  <li>Grant domain admin privileges to the new user.
    <ol>
      <li>Right-click the user <code>jane_admin</code> and select <strong>Properties</strong>.</li>
      <li>Navigate to the <strong>Member Of</strong> tab.</li>
      <li>Click <strong>Add</strong>, type <code>Domain Admins</code>, and click <strong>Check Names</strong>.</li>
      <li>Click <strong>OK</strong>, then <strong>Apply</strong>, and finally <strong>OK</strong> again.</li>
    </ol>
  </li>
  <li>Log out and disconnect from the DC virtual machine.</li>
  <li>Log back in using the new domain admin credentials.
    <ol>
      <li>Use the following login format: <code>mydomain.com\jane_admin</code></li>
    </ol>
  </li>
  <li>From this point forward in the lab, use the <code>jane_admin</code> account for all administrative tasks.</li>
</ol>
</p>
<br />

<p>
<img src="https://i.imgur.com/wsNcu9c.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <h3>Joining the Client Virtual Machine to the Domain</h3>
<ol>
  <li>Log in to the <strong>Client virtual machine</strong>.</li>
  <li>Right-click the <strong>Start Menu</strong> and select <strong>System</strong>.</li>
  <li>In the <strong>System Settings</strong> window, locate the <strong>Related settings</strong> section on the right-hand side and click <strong>"Rename this PC (advanced)"</strong>.</li>
  <li>Under the <strong>Computer Name</strong> tab, click <strong>Change</strong>.</li>
  <li>In the dialog box that appears:
    <ol>
      <li>Under the <strong>Member of</strong> section, select <strong>Domain</strong>.</li>
      <li>Enter your domain name. For this lab, use: <code>mydomain.com</code>.</li>
    </ol>
  </li>
  <li>Click <strong>OK</strong>. A <strong>Windows Security</strong> prompt will appear requesting domain credentials.</li>
  <li>Enter the administrator username and password created earlier (e.g., <code>jane_admin</code>).</li>
  <li>Once authenticated, a confirmation message will appear: <em>"Welcome to the [domain_name] domain."</em></li>
  <li>Click <strong>OK</strong>, then restart the VM when prompted. The system will automatically reboot.</li>
  <li>After the restart, log in using the domain admin account if not already logged in. For example: <code>mydomain.com\jane_admin</code>.</li>
  <li>Open <strong>Active Directory Users and Computers</strong> from the Start Menu.</li>
  <li>In the left panel, expand the domain (e.g., <code>mydomain.com</code>) and navigate to the <strong>Computers</strong> container.</li>
  <li>Verify that the <strong>Client VM</strong> appears in the list, confirming that it has successfully joined the domain.</li>
  <li>(Optional – for organizational purposes): 
    <ol>
      <li>Create a new <strong>Organizational Unit (OU)</strong> named <code>_CLIENTS</code>.</li>
      <li>Click and drag the Client VM into this new OU.</li>
      <li>Logout</li>
    </ol>
  </li>
</ol>
</p>
<br />

<p>
<img src="https://i.imgur.com/zpZpjvP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <h3>Setting up Remote Desktop to non-administrative users in the Client Virtual Machine</h3>
<ol>
  <li>Log in to the <strong>Client VM</strong> using the domain administrator account: <code>mydomain.com\jane_admin</code>.</li>
  <li>Right-click the <strong>Start Menu</strong> and select <strong>System</strong>.</li>
  <li>On the right-hand panel under <strong>Related settings</strong>, click <strong>Remote Desktop</strong>.</li>
  <li>In the <strong>Remote Desktop</strong> settings, scroll to the <strong>User accounts</strong> section and click <strong>"Select users that can remotely access this PC"</strong>.</li>
  <li>In the dialog box, click <strong>Add</strong>.</li>
  <li>Type <code>Domain Users</code> into the text box.</li>
  <li>Click <strong>Check Names</strong> to validate the group name.</li>
  <li>Click <strong>OK</strong>, and then click <strong>OK</strong> again to save the configuration.</li>
  <li>Logout</li>
</ol>
</p>
<br />

<p>
<img src="https://i.imgur.com/gow6d6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <h3>Bulk User Account Creation via PowerShell Script and Login Verification</h3>
<ol>
  <li>Log in to the <strong>Domain Controller (DC) VM</strong> using the admin account: <code>mydomain.com\jane_admin</code>.</li>
  <li>Open the <strong>Start Menu</strong>, search for <strong>Windows PowerShell ISE</strong>, right-click the application, and select <strong>Run as administrator</strong>.</li>
  <li>Once PowerShell ISE is open, click on <strong>View</strong> in the top menu and select <strong>Show Script Pane</strong>.</li>
  <li>Open the following link in a new browser tab: 
    <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1" target="_blank">
      Generate-Names-Create-Users.ps1
    </a>
  </li>
  <li>Copy the contents of the script and paste them into a new file in PowerShell ISE. Save the script file.</li>
  <li>Before running the script, press <strong>Ctrl + F</strong> to search for the word <code>Path</code> in the script.</li>
  <li>Verify that the <strong>OU path</strong> used in the script matches the folder name created earlier in the lab.
    <ol>
      <li>For this lab, the OU folder is named <strong>_EMPLOYEES</strong>.</li>
      <li>Ensure the name is spelled correctly with no typos or extra spaces.</li>
    </ol>
  </li>
  <li>Click the <strong>green play button</strong> at the top of PowerShell ISE to run the script.</li>
  <li>The script will generate multiple user accounts with randomized usernames. Each will be assigned the password <code>Password1</code>.</li>
  <li>Open <strong>Active Directory Users and Computers</strong> and navigate to the <strong>_EMPLOYEES</strong> OU folder to confirm the user accounts were created.</li>
  <li>Choose any of the newly created users and log in to the <strong>Client VM</strong> using the following format:
    <ol>
      <li>Username format: <code>mydomain.com\random_username</code></li>
      <li>Password: <code>Password1</code></li>
    </ol>
  </li>
  <li>After a successful login, log out of the Client VM.</li>
</ol>
</p>
<br />

