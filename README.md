<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hybrid Windows Server & Entra ID Lab</title>
</head>
<body>

<header>
  <h1>🌐 Hybrid Windows Server & Entra ID Lab Project</h1>
  <p>Enterprise Active Directory | Entra ID Sync | Group Policy | File Server RBAC</p>
</header>

<section>
  <h2>📌 Project Overview</h2>
  <p>
    This project demonstrates hands-on experience building a hybrid enterprise environment 
    combining <strong>Windows Server 2025 Active Directory</strong> with <strong>Microsoft Entra ID</strong> 
    (Azure AD). The lab simulates a real corporate IT infrastructure with on-premises identity 
    management synchronized to the cloud, complete with security policies, file server access control, 
    and modern authentication.
  </p>
  <p>
    <strong>Key Focus:</strong> Bridging traditional on-premises infrastructure with modern cloud 
    identity security—essential skills for Junior System Administrator and IAM Analyst roles.
  </p>
</section>

<section>
  <h2>🏗️ Lab Environment & Architecture</h2>
  <ul>
    <li><strong>Windows Server 2025</strong> - Domain Controller (DC01)</li>
    <li><strong>Active Directory Domain Services (AD DS)</strong></li>
    <li><strong>File Server (FILESERVER)</strong> with role-based access control</li>
    <li><strong>Windows 11 Clients</strong> - CLIENT1 & CLIENT2 (domain-joined)</li>
    <li><strong>Microsoft Entra ID</strong> (cloud identity)</li>
    <li><strong>Entra Connect</strong> - Hybrid identity synchronization</li>
    <li><strong>Oracle VirtualBox</strong> - Virtualization platform</li>
  </ul>
  
  <h3>Network Configuration</h3>
  <ul>
    <li>Domain: <code>crop.Lanitc</code></li>
    <li>Internal Network: 172.16.0.0/24</li>
    <li>DC01: 172.16.0.1 (DNS/Gateway)</li>
    <li>FILESERVER: RBAC-enabled file shares</li>
    <li>Clients: Domain-joined Windows 11 workstations</li>
  </ul>
</section>

<section>
  <h2>🖥️ Infrastructure Setup</h2>
  
  <h3>1️⃣ Windows Server 2025 Installation</h3>
  <ul>
    <li>Installed Windows Server 2025 Standard Evaluation</li>
    <li>Configured static IP addressing</li>
    <li>Promoted server to Domain Controller</li>
    <li>Created <strong>crop.Lanitc</strong> Active Directory domain</li>
  </ul>
  <img src="1.png" alt="Windows Server 2025 Installation - Edition Selection">
  <img src="2.png" alt="Active Directory Domain - crop.Lanitc Created">
  
  <h3>2️⃣ Domain-Joined Clients</h3>
  <ul>
    <li>Deployed 2 Windows 11 VMs (CLIENT1, CLIENT2)</li>
    <li>Successfully joined to <code>crop.Lanitc</code> domain</li>
    <li>Configured domain user authentication</li>
    <li>Verified domain connectivity and Group Policy application</li>
  </ul>
  <img src="3.png" alt="Windows 11 Client - Domain Join Confirmation">
  <img src="4.png" alt="Active Directory - CLIENT1 and CLIENT2 Domain Computers">
</section>

<section>
  <h2>👥 Active Directory Configuration</h2>
  
  <h3>Organizational Unit (OU) Structure</h3>
  <ul>
    <li>Created <strong>UserGR</strong> OU for departmental organization</li>
    <li>Sub-OUs: <strong>Finance</strong>, <strong>HR</strong>, <strong>Users</strong></li>
    <li>Organized computers in <strong>Computers</strong> OU</li>
    <li>Separated servers in <strong>Servers</strong> OU (FILESERVER)</li>
  </ul>
  
  <h3>Security Groups & RBAC</h3>
  <ul>
    <li>Created <strong>Finance_Users</strong> security group</li>
    <li>Created <strong>HR_Users</strong> security group</li>
    <li>Created <strong>IT_Users</strong> security group</li>
    <li>Implemented role-based access control for file shares</li>
  </ul>
  <img src="5.png" alt="Active Directory - FILESERVER Computer Object">
  <img src="6.png" alt="Security Groups - Finance_Users, HR_Users, IT_Users">
</section>

<section>
  <h2>📂 File Server & Access Control</h2>
  
  <h3>Shared Folder Structure</h3>
  <ul>
    <li><code>\\FILESERVER\Finance</code> - Finance department files</li>
    <li><code>\\FILESERVER\HR</code> - Human Resources files</li>
    <li><code>\\FILESERVER\Public</code> - Company-wide shared files</li>
  </ul>
  
  <h3>NTFS Permissions (Role-Based Access Control)</h3>
  <table border="1" cellpadding="8" cellspacing="0">
    <tr>
      <th>Share</th>
      <th>Security Group</th>
      <th>Permissions</th>
    </tr>
    <tr>
      <td>\\FILESERVER\Finance</td>
      <td>Finance_Users</td>
      <td>Modify, Read, Write</td>
    </tr>
    <tr>
      <td>\\FILESERVER\HR</td>
      <td>HR_Users</td>
      <td>Modify, Read, Write</td>
    </tr>
    <tr>
      <td>\\FILESERVER\Public</td>
      <td>Domain Users</td>
      <td>Read Only</td>
    </tr>
  </table>
  
  <img src="7.png" alt="File Shares - net share Command Output">
  
  <h3>Access Control Testing</h3>
  <ul>
    <li>✅ Finance users can access Finance share</li>
    <li>✅ HR users can access HR share</li>
    <li>❌ Finance users CANNOT access HR share (security verified)</li>
    <li>✅ All domain users can read Public share</li>
  </ul>
</section>

<section>
  <h2>⚙️ Group Policy Objects (GPO)</h2>
  
  <h3>Drive Mapping Policy</h3>
  <ul>
    <li>Created GPO: <strong>"Drive mapping"</strong></li>
    <li>Auto-maps <code>H:</code> drive to user's department share</li>
    <li>Applied to <strong>UserGR</strong> OU</li>
    <li>Uses item-level targeting based on security groups</li>
  </ul>
  <img src="8.png" alt="Group Policy Results - Drive Mapping GPO Applied">
  
  <h3>Security Policies Configured</h3>
  <ul>
    <li>Password complexity requirements</li>
    <li>Account lockout policies</li>
    <li>Screen lock timeout settings</li>
    <li>Windows Firewall enforcement</li>
  </ul>
  <img src="9.png" alt="Mapped Network Drive - H: Drive to \\Fileserver\HR">
  
  <h3>Password Policy Enforcement</h3>
  <ul>
    <li>Minimum password length: 8 characters</li>
    <li>Complexity requirements: Enabled</li>
    <li>Password history: 24 passwords remembered</li>
    <li>Account lockout threshold: 5 invalid attempts</li>
  </ul>
  <img src="10.png" alt="Password Policy Error - Complexity Requirements Enforced">
</section>

<section>
  <h2>☁️ Hybrid Identity - Microsoft Entra ID Integration</h2>
  
  <h3>Entra Connect Setup</h3>
  <ul>
    <li>Downloaded and installed <strong>Microsoft Entra Connect</strong> on DC01</li>
    <li>Configured <strong>Password Hash Synchronization</strong></li>
    <li>Connected on-premises AD (<code>crop.Lanitc</code>) to Entra ID</li>
    <li>Enabled <strong>Seamless Single Sign-On (SSO)</strong></li>
  </ul>
  <img src="11.png" alt="Entra Connect - Welcome Screen">
  <img src="12.png" alt="Entra Connect - Directory Connection crop.Lanitc">
  
  <h3>OU Filtering Configuration</h3>
  <ul>
    <li>Selected specific OUs to sync: <strong>Finance</strong>, <strong>HR</strong>, <strong>Users</strong></li>
    <li>Excluded computer and server objects from cloud sync</li>
    <li>Implemented selective synchronization for security</li>
  </ul>
  <img src="13.png" alt="OU Filtering - Finance, HR, Users Selected">
  
  <h3>Single Sign-On Configuration</h3>
  <ul>
    <li>Configured Seamless SSO for <code>crop.Lanitc</code> domain</li>
    <li>Enabled automatic cloud authentication for domain users</li>
    <li>Entered domain admin credentials for SSO setup</li>
  </ul>
  <img src="14.png" alt="Single Sign-On - Enter Domain Admin Credentials">
  
  <h3>Synchronization Complete</h3>
  <ul>
    <li>✅ Entra Connect configuration succeeded</li>
    <li>✅ Synchronization process initiated</li>
    <li>✅ Password Hash Sync enabled</li>
    <li>✅ Password writeback configured (for SSPR)</li>
    <li>✅ mS-DS-ConsistencyGuid used as source anchor</li>
  </ul>
  <img src="15.png" alt="Configuration Complete - Sync Process Initiated">
</section>

<section>
  <h2>🔐 Cloud Identity Verification</h2>
  
  <h3>Synced Users in Entra ID</h3>
  <ul>
    <li>Successfully synced <strong>8 users</strong> to Microsoft Entra ID</li>
    <li>All users show <strong>"On-premises sync enabled: Yes"</strong></li>
    <li>Verified source: <strong>Windows Server AD</strong></li>
    <li>UPN mapping: <code>user@crop.Lanitc</code> → <code>user@Lanitcs.onmicrosoft.com</code></li>
  </ul>
  <img src="16.png" alt="Azure Portal - 8 Users Synced from On-Premises AD">
  
  <h3>User Sync Details</h3>
  <ul>
    <li><strong>On-premises sync enabled:</strong> Yes</li>
    <li><strong>Last sync date:</strong> Feb 13, 2026, 5:32 PM</li>
    <li><strong>Distinguished Name:</strong> CN=joe doe,OU=Finance,OU=UserGR,DC=crop,DC=Lanitc</li>
    <li><strong>On-premises domain:</strong> crop.Lanitc</li>
    <li><strong>SAM account name:</strong> Jdoe</li>
  </ul>
  <img src="17.png" alt="User Profile - On-Premises Sync Details and Distinguished Name">
</section>

<section>
  <h2>🎯 Skills Demonstrated</h2>
  
  <h3>System Administration</h3>
  <ul>
    <li>✅ Windows Server 2025 deployment and configuration</li>
    <li>✅ Active Directory Domain Services (AD DS) setup</li>
    <li>✅ DNS and DHCP configuration</li>
    <li>✅ File server administration with shared folders</li>
    <li>✅ Client workstation domain integration</li>
    <li>✅ Virtualization with Oracle VirtualBox</li>
  </ul>
  
  <h3>Identity & Access Management (IAM)</h3>
  <ul>
    <li>✅ Organizational Unit (OU) structure design</li>
    <li>✅ Security group creation and management</li>
    <li>✅ Role-Based Access Control (RBAC) implementation</li>
    <li>✅ NTFS and share permission configuration</li>
    <li>✅ Hybrid identity architecture (on-prem + cloud)</li>
    <li>✅ Microsoft Entra Connect configuration</li>
    <li>✅ Password Hash Synchronization</li>
    <li>✅ Seamless Single Sign-On (SSO)</li>
    <li>✅ Identity lifecycle management</li>
  </ul>
  
  <h3>Group Policy Management</h3>
  <ul>
    <li>✅ GPO creation and configuration</li>
    <li>✅ Drive mapping automation</li>
    <li>✅ Security policy enforcement (passwords, lockout)</li>
    <li>✅ Item-level targeting with security groups</li>
    <li>✅ GPO troubleshooting (gpresult, gpupdate)</li>
  </ul>
  
  <h3>Security & Compliance</h3>
  <ul>
    <li>✅ Password complexity policies</li>
    <li>✅ Account lockout protection</li>
    <li>✅ File-level access control</li>
    <li>✅ Least privilege principle implementation</li>
    <li>✅ Secure cloud identity synchronization</li>
  </ul>
</section>

<section>
  <h2>🔄 Complete Identity Lifecycle</h2>
  <p>
    This lab demonstrates the <strong>end-to-end enterprise identity lifecycle</strong>:
  </p>
  <p><strong>
    Infrastructure → Domain → OUs → Users → Groups → Permissions → GPO → 
    Cloud Sync → Hybrid Identity → Monitoring
  </strong></p>
  
  <h3>User Journey Example</h3>
  <ol>
    <li>User account created in Finance OU</li>
    <li>Added to Finance_Users security group</li>
    <li>Receives access to \\FILESERVER\Finance share</li>
    <li>Drive mapping GPO applies H: drive automatically</li>
    <li>Account syncs to Microsoft Entra ID via Entra Connect</li>
    <li>User can now access cloud services with same credentials</li>
    <li>Single Sign-On provides seamless authentication</li>
  </ol>
</section>

<section>
  <h2>🧪 Testing & Validation</h2>
  
  <h3>Access Control Tests</h3>
  <ul>
    <li>✅ Finance user successfully accesses Finance share</li>
    <li>✅ HR user successfully accesses HR share</li>
    <li>❌ Cross-department access properly denied</li>
    <li>✅ Public share readable by all domain users</li>
  </ul>
  
  <h3>Group Policy Validation</h3>
  <ul>
    <li>✅ Drive mapping GPO applies successfully</li>
    <li>✅ H: drive mapped to correct department share</li>
    <li>✅ Password policies enforced (tested with weak password)</li>
    <li>✅ Account lockout working after 5 failed attempts</li>
  </ul>
  
  <h3>Hybrid Identity Verification</h3>
  <ul>
    <li>✅ All users synced to Entra ID (8 users)</li>
    <li>✅ "On-premises sync enabled: Yes" confirmed</li>
    <li>✅ Last sync timestamp verified (Feb 13, 2026)</li>
    <li>✅ Distinguished names properly mapped</li>
  </ul>
  
  <h3>Commands Used</h3>
  <ul>
    <li><code>gpupdate /force</code> - Force Group Policy refresh</li>
    <li><code>gpresult /r</code> - View applied GPOs</li>
    <li><code>net share</code> - Verify file shares</li>
    <li><code>Start-ADSyncSyncCycle</code> - Force Entra Connect sync</li>
    <li><code>Get-ADSyncScheduler</code> - Check sync status</li>
  </ul>
</section>

<section>
  <h2>🛠️ Troubleshooting & Problem Solving</h2>
  
  <h3>Issues Encountered & Solutions</h3>
  <ul>
    <li>
      <strong>Issue:</strong> Drive mappings not appearing for users<br>
      <strong>Solution:</strong> Changed GPO security filtering to "Authenticated Users" 
      with item-level targeting for specific groups
    </li>
    <li>
      <strong>Issue:</strong> Entra Connect - "Enterprise Admin account not allowed"<br>
      <strong>Solution:</strong> Used "Create new AD account" option to let Entra Connect 
      create MSOL service account with minimum permissions
    </li>
    <li>
      <strong>Issue:</strong> IE Enhanced Security blocking Entra Connect setup<br>
      <strong>Solution:</strong> Disabled IE ESC via Server Manager for Administrators
    </li>
    <li>
      <strong>Issue:</strong> Network discovery disabled on clients<br>
      <strong>Solution:</strong> Enabled network discovery and file sharing in 
      Network and Sharing Center
    </li>
  </ul>
</section>

<section>
  <h2>📚 Key Learning Outcomes</h2>
  
  <p><strong>This project has strengthened my readiness for:</strong></p>
  <ul>
    <li><strong>Junior System Administrator</strong> roles</li>
    <li><strong>IAM Analyst</strong> positions</li>
    <li><strong>IT Support Engineer</strong> (Tier 2/3)</li>
    <li><strong>Identity Engineer</strong> roles</li>
  </ul>
  
  <p><strong>By demonstrating:</strong></p>
  <ul>
    <li>✅ Infrastructure management (servers, networking, virtualization)</li>
    <li>✅ Identity & Access Management expertise</li>
    <li>✅ Hybrid cloud architecture implementation</li>
    <li>✅ Security policy enforcement</li>
    <li>✅ Troubleshooting complex IT issues</li>
    <li>✅ Professional documentation skills</li>
  </ul>
</section>

<section>
  <h2>🚀 Future Enhancements</h2>
  
  <h3>Phase 2 (Planned)</h3>
  <ul>
    <li>Multi-Factor Authentication (MFA) enforcement</li>
    <li>Self-Service Password Reset (SSPR) with password writeback</li>
    <li>Conditional Access Policies</li>
    <li>Privileged Identity Management (PIM)</li>
    <li>Azure AD Application Proxy</li>
  </ul>
  
  <h3>Phase 3 (Advanced)</h3>
  <ul>
    <li>Deploy secondary Domain Controller for redundancy</li>
    <li>Implement DFS (Distributed File System)</li>
    <li>Setup WSUS (Windows Server Update Services)</li>
    <li>Configure Certificate Services (PKI)</li>
    <li>Disaster recovery and backup strategies</li>
  </ul>
</section>

<footer>
  <p>
    <strong>Built as part of hands-on learning for Junior System Administrator 
    and IAM Analyst roles.</strong>
  </p>
  <p>
    This lab demonstrates the integration of traditional on-premises Active Directory 
    with modern cloud identity services—a critical skillset for today's hybrid enterprise 
    environments.
  </p>
</footer>

</body>
</html>
