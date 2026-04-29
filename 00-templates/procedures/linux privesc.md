## 1 - Enumeration
### Users and Groups

- List local users and local groups.
- Identify currently logged-on users.
- Check last login history.
- Check membership of high-value groups: `wheel`, `docker`, `shadow`, `lxc`/`lxd`, `disk`, `adm`.
### Operating System

- Identify the Linux distribution and kernel version.
- Identify the system architecture (32-bit or 64-bit).
- Check if the host is joined to an Active Directory domain.
### Network

- Enumerate IP addresses and network interfaces.
- List active connections and listening ports.
### Processes and Programs

- Enumerate running processes and installed programs.

---
## Privilege Escalation - Global Checks

Run these regardless of which user account is held.

- Identify the Linux distribution and kernel version.
- Check for credentials in web application configuration files.
- Check interesting directories (`/opt`, `/var/mail`, `/srv`, `/var/www`, etc.).
- Check capabilities (`getcap -r / 2>/dev/null`).
- Check if the `sudo` version is vulnerable (CVE-2023-22809).
- Run an internal Nmap scan to discover services not exposed externally.
- Check for PwnKit (CVE-2021-4034).
- Check the Logrotate version (vulnerable: 3.8.8, 3.11.0, 3.15.0, 3.18.0).
- Monitor processes for a few minutes. Look for scripts or binaries running as root.
- Look for writable Docker socket files (`/var/run/docker.sock`).
- Look for hijackable Tmux sessions owned by privileged users.
- Check for NFS shares with `no_root_squash` enabled (`/etc/exports`).
- Check for applicable kernel exploits (DirtyCow CVE-2016-5195, DirtyPipe CVE-2022-0847).
- Capture traffic with `tcpdump` and look for cleartext credentials.

---

## Privilege Escalation - Per User Checks

Repeat for every newly compromised user account.

- Check which groups the user belongs to.
- Check `sudo` rights (`sudo -l`).
- Check for interesting environment variables (`env`).
- Look for SSH keys in the home directory.
- Check for hidden files in the home directory.
- Check shell history files (`.bash_history`, `.zsh_history`, etc.).
- Hunt for interesting files owned by or readable by the user.
- Enumerate SUID/SGID binaries and cross-reference against GTFOBins.
- Check for exploitable cron jobs:
    - System-wide cron jobs (`/etc/crontab`, `/etc/cron.d/`).
    - User-specific cron jobs (`crontab -l`).
    - Monitor processes for regularly repeating commands suggesting a hidden cron job.
- Attempt to read other users' home directories (`.ssh/id_rsa`, `.bash_history`, etc.).
- Try the current user's password for other local accounts and for `sudo`.
- Run `linpeas.sh` and review output.