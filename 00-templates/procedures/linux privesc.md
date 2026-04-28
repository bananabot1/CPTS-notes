## Enumeration[](https://field-manual.brunorochamoura.com/manual/procedures/linux-enumeration--privilege-escalation/#enumeration)

- Enumerate users and groups
    - [ ]  List local users
    - [ ]  List local groups
    - [ ]  Currently logged on users
    - [ ]  Last logins
    - Check membership of interesting groups
        - [ ]  `wheel`
        - [ ]  `docker`
        - [ ]  `shadow`
        - [ ]  `lxc` and `lxd`
        - [ ]  `disk`
        - [ ]  `adm`
- Enumerate operating system information
    - [ ]  Linux distribution
    - [ ]  Kernel Version
    - [ ]  Architecture (32 or 64 bit)
    - [ ]  Is it AD domain joined?
- Enumerate network information
    - [ ]  IP addresses and network interfaces
    - [ ]  List active connections and listening ports
- Enumerate program and processes information

## Privilege Escalation[](https://field-manual.brunorochamoura.com/manual/procedures/linux-enumeration--privilege-escalation/#privilege-escalation)

### Global[](https://field-manual.brunorochamoura.com/manual/procedures/linux-enumeration--privilege-escalation/#global)

- [ ]  Identify the Linux distribution and Kernel version
- [ ]  Check for credentials in web application configuration files
- [ ]  Check interesting directories (e.g. `/opt`, `/var/mail`, etc.)
- [ ]  Check capabilities
- [ ]  Check if `sudo` version is vulnerable ([CVE-2023–22809](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2023-22809))
- [ ]  Internal Nmap scan
- [ ]  Check PwnKit
- [ ]  Check LogRotate (versions 3.8.8, 3.11.0, 3.15. and 3.18.0)
- [ ]  Monitor processes. Look for anything interesting.
- [ ]  Look for writable Docker socket files.
- [ ]  Look for Tmux sessions that can be hijacked
- [ ]  Check for NFS shares with `no_root_squash` enabled
- [ ]  Check kernel exploits (e.g. DirtyCow, DirtyPipe)
- [ ]  Listen to traffic using TcpDump. Any cleartext credential?

### Per User[](https://field-manual.brunorochamoura.com/manual/procedures/linux-enumeration--privilege-escalation/#per-user)

- [ ]  Check which groups user belongs to
- [ ]  Check `sudo` rights
- [ ]  Check for environment variables
- [ ]  Look for ssh keys on home directory
- [ ]  Check for hidden files in home directory
- [ ]  Check history files on home directory
- [ ]  Hunt for interesting files
- [ ]  Enumerate SUID / GUID binaries
- Check for exploitable cronjobs:
    - [ ]  System-wide cronjobs
    - [ ]  User-specific cronjobs
    - [ ]  Monitor processes for regularly repeating commands, suggesting a hidden cron job.
- [ ]  Try to read other user’s home directory (`.ssh/id_rsa`, `.bash_history`, etc.)
- [ ]  Try using user’s password for other users
- [ ]  Run `linpeas.sh`