<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>PowerShell Automation Project</h1>

<h2>Environments and Technologies Used</h2>

- Hyper-V Manager Virtual Machine
- Active Directory Domain Services
- PowerShell
- Notepad/Notes App (Needed for saving usernames and passwords)

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10

<h2>Prepare Virtual Machine and Domain Controller for this project </h2>

- Create and configure Domain Controller(DC) virtual machine and Windows virtual machine
- Set up Active Directory Domain Services within the DC virtual machine.
- Set the client VM's DNS settings to the Domain Controller(DC) VM's private IP Address, then restart the VM from Azure
- Log into the Client VM and attempt to ping the Domain Controller's private IP Address, then run "ipconfig /all") within PowerShell as an admin
- Finally, the expected result should be the IP Address of the Domain Controller VM

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

