In today's security-conscious world, most applications wisely use TLS to encrypt sensitive data in transit. However, not all environments are fully secured. Legacy systems, misconfigured services, or test applications launched without HTTPS can still result in the use of unencrypted protocols such as HTTP or SNMP. These gaps present a valuable opportunity for attackers: `the chance to hunt for credentials in cleartext network traffic`. 
The table below lists several common protocols alongside their encrypted counterparts. While it is now more common to encounter the secure versions, there was a time when plaintext protocols were widely used.

|Unencrypted Protocol|Encrypted Counterpart|Description|
|---|---|---|
|`HTTP`|`HTTPS`|Used for transferring web pages and resources over the internet.|
|`FTP`|`FTPS/SFTP`|Used for transferring files between a client and a server.|
|`SNMP`|`SNMPv3 (with encryption)`|Used for monitoring and managing network devices like routers and switches.|
|`POP3`|`POP3S`|Retrieves emails from a mail server to a local client.|
|`IMAP`|`IMAPS`|Accesses and manages email messages directly on the mail server.|
|`SMTP`|`SMTPS`|Sends email messages from client to server or between mail servers.|
|`LDAP`|`LDAPS`|Queries and modifies directory services like user credentials and roles.|
|`RDP`|`RDP (with TLS)`|Provides remote desktop access to Windows systems.|
|`DNS (Traditional)`|`DNS over HTTPS (DoH)`|Resolves domain names into IP addresses.|
|`SMB`|`SMB over TLS (SMB 3.0)`|Shares files, printers, and other resources over a network.|
|`VNC`|`VNC with TLS/SSL`|Allows graphical remote control of another computer.|

[Wireshark](https://www.wireshark.org/) is a well-known packet analyzer that comes pre-installed in nearly all penetration testing Linux distributions. It features a powerful [filter engine](https://www.wireshark.org/docs/man-pages/wireshark-filter.html) that allows for efficient searching through both live and captured network traffic. 

---

## Wireshark Filters

| Wireshark filter                                  | Description                                                                                                                                                                          |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `ip.addr == 56.48.210.13`                         | Filters packets with a specific IP address                                                                                                                                           |
| `tcp.port == 80`                                  | Filters packets by port (HTTP in this case).                                                                                                                                         |
| `http`                                            | Filters for HTTP traffic.                                                                                                                                                            |
| `dns`                                             | Filters DNS traffic, which is useful to monitor domain name resolution.                                                                                                              |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0`        | Filters SYN packets (used in TCP handshakes), useful for detecting scanning or connection attempts.                                                                                  |
| `icmp`                                            | Filters ICMP packets (used for Ping), which can be useful for reconnaissance or network issues.                                                                                      |
| `http.request.method == "POST"`                   | Filters for HTTP POST requests. In the case that POST requests are sent over unencrypted HTTP, it may be the case that passwords or other sensitive information is contained within. |
| `tcp.stream eq 53`                                | Filters for a specific TCP stream. Helps track a conversation between two hosts.                                                                                                     |
| `eth.addr == 00:11:22:33:44:55`                   | Filters packets from/to a specific MAC address.                                                                                                                                      |
| `ip.src == 192.168.24.3 && ip.dst == 56.48.210.3` | Filters traffic between two specific IP addresses. Helps track communication between specific hosts.                                                                                 |