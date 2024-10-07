# Windows Server Documentation

## Forensics Questions Answers
### Check for network shares: Using Computer Management
1. Right-click on the **Start** button and select **Computer Management**.
2.  Expand **Shared Folders** in the left pane. 
3.  Click on **Shares**. This will display all the shared folders on the server, including their local paths. 

### Find the SHA 256 hash of a file: Using Powershell
1. Open **Powershell**
2. Use the following command, replacing <PathToYourFile> with the actual path to your file: 
```
Get-FileHash <PathToYourFile> -Algorithm SHA256 
```
## Normal Answers

### Enable auto updates: Using Group Policy
1. **Open Group Policy Editor:**
   - Press `Windows + R`, type `gpedit.msc`, and press Enter.

2. **Navigate to Windows Update Settings:**
   - Go to **Computer Configuration > Administrative Templates > Windows Components > Windows Update**.

3. **Configure Automatic Updates:**
   - Find **Configure Automatic Updates** and double-click it.

### Remove unwanted programs
   - Ex. Wireshark, Nocap, Bit Torrent

### Enable Firewall

1. **Open Group Policy Editor:**
   - Press `Windows + R`, type `gpedit.msc`, and press Enter.

2. **Navigate to Windows Update Settings:**
   - Go to **Computer Configuration > Windows Settings > Security Settings > Windows Defender Firewall With Advanced Security > Windows Defender Firewall With Advanced Security – Local Group Policy Object**.

3. **Enable Policies:**

   - **Windows Defender Firewall Properties:**
     - Firewall State = On
     - Inbound Connections = Block (default)
     - Outbound Connections = Allow (default)

### Password Settings

1. **Open Group Policy Editor:**
   - Press `Windows + R`, type `gpedit.msc`, and press Enter.

2. **Navigate to Password Policy:**
   - Go to **Computer Configuration > Windows Settings > Security Settings > Account Policies > Password Policies**.

3. **Enable Policies:**

   - **Policies:**
     - **Enforce Password History** = 10
     - **Maximum Password Age** = 90
     - **Minimum Password Age** = 1
     - **Minimum Password Length** = 14
     - **Minimum Password Length Audit** = 14
     - **Password Must Meet Complexity Requirements** = Enabled
     - **Store Passwords Using Reversible Encryption** = Disabled

### Account Lockout Settings

1. **Open Group Policy Editor:**
   - Press `Windows + R`, type `gpedit.msc`, and press Enter.

2. **Navigate to Password Policy:**
   - Go to **Computer Configuration > Windows Settings > Security Settings > Account Policies > Password Policies**.

3. **Enable Policies:**
   - **Policies:**

### Remove / add users
1. Open Users:
   - Type **"users"** in the windows search bar
   - Open **"Add,edit, or remove other users"**

### Method 1: Using the Services Console

1. **Open Services:**
   - Press `Windows + R`, type `services.msc`, and press Enter.

2. **Locate the Event Log Service:**
   - In the Services window, scroll down to find **Windows Event Log**.

3. **Check Status:**
   - The Status column will show if the service is **Running**. If the Startup type is **Automatic**, then Start.

### Remove Unwanted Shares

1. **Open Services:**
   - Press `Windows + R`, type `fsmgmt.msc`, and press Enter.

2. **Find Unwanted User:**
   - Example: greybeard: Right-click and select **Stop Sharing**.

### Add Passwords to Users

1. How to Detect the Problem:

   - Look at **User Accounts** under the Control Panel.

### How to Change Password:

1. Go to **Control Panel > User Accounts > Manage Accounts**.
2. Pick the account that doesn't say “password protected,” then click **Change the password**.

### Enable: Limit Local Use of Blank Passwords to Console Only

1. **Open Services:**
   - Press `Windows + R`, type `secpol.msc`, and press Enter.

2. **Locate the Setting:**
   - Go to **Security Setting > Local Policies > Security Options**.

3. **Find the Setting:**
   - Look for **Accounts: Limit local use of blank passwords to console only**.
   - Right-click **Properties**, then click **Enable** and apply.

### Disable: FTP Services

1. **Open Services:**
   - Press `Windows + R`, type **Services**, and press Enter.

2. **Locate Microsoft FTP Service:**
   - Scroll until you find **Microsoft FTP Service**. Right-click, select **Properties**, then choose **Disable** and **Stop**, and apply the changes.
