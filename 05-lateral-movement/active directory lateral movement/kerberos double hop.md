**Overview:**
There's an issue known as the "Double Hop" problem that arises when an attacker attempts to use Kerberos authentication across two (or more) hops. The issue concerns how Kerberos tickets are granted for specific resources. Kerberos tickets should not be viewed as passwords. They are signed pieces of data from the KDC that state what resources an account can access. When we perform Kerberos authentication, we get a "ticket" that permits us to access the requested resource (i.e., a single machine). On the contrary, when we use a password to authenticate, that NTLM hash is stored in our session and can be used elsewhere without issue.
The "Double Hop" problem often occurs when using WinRM/Powershell since the default authentication mechanism only provides a ticket to access a specific resource. This will likely cause issues when trying to perform lateral movement or even access file shares from the remote shell. In this situation, the user account being used has the rights to perform an action but is denied access. The most common way to get shells is by attacking an application on the target host or using credentials and a tool such as PSExec. In both of these scenarios, the initial authentication was likely performed over SMB or LDAP, which means the user's NTLM Hash would be stored in memory. Sometimes we have a set of credentials and are restricted to a particular method of authentication, such as WinRM, or would prefer to use WinRM for any number of reasons.

The crux of the issue is that when using WinRM to authenticate over two or more connections, the user's password is never cached as part of their login. 
A few workarounds for the double-hop issue are covered in [this post](https://posts.slayerlabs.com/double-hop/). We can use a "nested" `Invoke-Command` to send credentials (after creating a PSCredential object) with every request, so if we try to authenticate from our attack host to host A and run commands on host B, we are permitted
: We cannot use `Register-PSSessionConfiguration` from an evil-winrm shell because we won't be able to get the credentials popup. Furthermore, if we try to run this by first setting up a PSCredential object and then attempting to run the command by passing credentials like `-RunAsCredential $Cred`, we will get an error because we can only use `RunAs` from an elevated PowerShell terminal. Therefore, this method will not work via an evil-winrm session as it requires GUI access and a proper PowerShell console. Furthermore, in our testing, we could not get this method to work from PowerShell on a Parrot or Ubuntu attack host due to certain limitations on how PowerShell on Linux works with Kerberos credentials. This method is still highly effective if we are testing from a Windows attack host and have a set of credentials or compromise a host and can connect via RDP to use it as a "jump host" to mount further attacks against hosts in the environment.

---
## PSCredential Object

After connecting to a remote host with domain credentials (winrm, we import PowerView and then try to run a command. As seen below, we get an error because we cannot pass our authentication on to the Domain Controller to query for the SPN accounts.
```
klist
```
check with `klist`, we see that we only have a cached Kerberos ticket for our current server.
```
$SecPassword = ConvertTo-SecureString '!qazXSW@' -AsPlainText -Force
```
set up a PSCredential object and try again. First, we set up our authentication.
```
get-domainuser -spn -credential $Cred | select samaccountname
```
try to query the SPN accounts using PowerView and are successful because we passed our credentials along with the command.
If we RDP to the same host, open a CMD prompt, and type `klist`, we'll see that we have the necessary tickets cached to interact directly with the Domain Controller, and we don't need to worry about the double hop problem. This is because our password is stored in memory, so it can be sent along with every request we make.

## Register PSSession Configuration
nother option to change our setup to be able to interact directly with the DC or other hosts/resources without having to set up a PSCredential object and include credentials along with every command (which may not be an option with some tools).
```
 Enter-PSSession -ComputerName ACADEMY-AEN-DEV01.INLANEFREIGHT.LOCAL -Credential inlanefreight\backupadm
```
start by first establishing a WinRM session on the remote host.

```
Register-PSSessionConfiguration -Name backupadmsess -RunAsCredential inlanefreight\backupadm
```
registering a new session configuration using the [Register-PSSessionConfiguration](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/register-pssessionconfiguration?view=powershell-7.2) cmdlet.
Once this is done, we need to restart the WinRM service by typing `Restart-Service WinRM` in our current PSSession. This will kick us out, so we'll start a new PSSession using the named registered session we set up previously.
We can now run tools such as PowerView without having to create a new PSCredential object.


