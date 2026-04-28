Live Host Enumeration

- Conduct a ping sweep on the IP range
- Use NetExec on the IP range (better information)
- Use Responder to catch IP addresses

Be sure to properly understand the role of each host. Do your service enumeration. User Enumeration With foothold

- Get user list via SMB Without foothold
    
- Attempt to get user list via SMB Null Authentication
- Attempt to get user list via LDAP Anonymous Bind
- Attempt to get user list via RPCClient
- Attempt to get user list via RID brute-forcing
- Attempt to get user list via Kerbruting

Some of these techniques are not guaranteed to discover all users. At least try the SMB, LDAP, RPCClient and RID methods. Get Foothold

- Find Kerberoastable users from the user list
- Find ASREProastable users from the user list
- Use Responder to catch credential hashes
- Try SMB Null Authentication to spider through SMB shares looking for credentials
- Get `SYSTEM` / `root` on Domain connected host to get a Computer account
- As a last resource, try password spraying with the user list

Password spraying can lock accounts due to repeated failed attempts and should be used cautiously. Attacks

- Use SharpHound to collect data to feed BloodHound
- Check compromised hosts on BloodHound for outbound attack paths
- Use NetExec to check for command execution via SMB, WinRM, and RDP for each compromised user.
- Kerberoast
- ASREProast
- Look for credentials in GPOs (`gpp_password`, `autologin`)
- For each compromised user, spider through readable SMB shares for sensitive information
- For each compromised user, conduct SMB Hash Theft attacks on writable SMB shares
- Look for passwords in user’s description fields
- Check the DC’s SYSVOL SMB share for scripts containing credentials
- [NoPac]([https://github.com/cube0x0/CVE-2021-1675](https://github.com/cube0x0/CVE-2021-1675))
- [PrintNightmare]([https://github.com/m8sec/CVE-2021-34527](https://github.com/m8sec/CVE-2021-34527))
- [PetitPotam]([https://github.com/topotam/PetitPotam](https://github.com/topotam/PetitPotam))
- Try compromised local administrator hashes on other hosts
- Try Responder on different hosts
- Look for users with the `PASSWD_NOTREQD` field
- Password spray using previously found passwords