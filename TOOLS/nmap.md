**Overview:**
- Network Mapper (`Nmap`) is an open-source network analysis and security auditing tool written in C, C++, Python, and Lua. 
- It is designed to scan networks and identify which hosts are available on the network using raw packets, and services and applications, including the name and version, where possible. It can also identify the operating systems and versions of these hosts. 
- Nmap also offers scanning capabilities that can determine if packet filters, firewalls, or intrusion detection systems (IDS) are configured as needed.
- Common use cases:
	- Audit the security aspects of networks
	- Simulate penetration tests
	- Check firewall and IDS settings and configurations
	- Types of possible connections
	- Network mapping
	- Response analysis
	- Identify open ports
	- Vulnerability assessment as well.
---


```
nmap <scan types> <options> <target>
```
nmap basic syntax
## scanning options

|                      |                                                                                                      |
| -------------------- | ---------------------------------------------------------------------------------------------------- |
| `10.10.10.0/24`      | Target network range.                                                                                |
| `-sn`                | Disables port scanning.                                                                              |
| `-Pn`                | Disables ICMP Echo Requests                                                                          |
| `-n`                 | Disables DNS Resolution.                                                                             |
| `-PE`                | Performs the ping scan by using ICMP Echo Requests against the target.                               |
| `--packet-trace`     | Shows all packets sent and received.                                                                 |
| `--reason`           | Displays the reason for a specific result.                                                           |
| `--disable-arp-ping` | Disables ARP Ping Requests.                                                                          |
| `--top-ports=<num>`  | Scans the specified top ports that have been defined as most frequent.                               |
| `-p-`                | Scan all ports.                                                                                      |
| `-p22-110`           | Scan all ports between 22 and 110.                                                                   |
| `-p22,25`            | Scans only the specified ports 22 and 25.                                                            |
| `-F`                 | Scans top 100 ports.                                                                                 |
| `-sS`                | Performs an TCP SYN-Scan.                                                                            |
| `-sA`                | Performs an TCP ACK-Scan.                                                                            |
| `-sU`                | Performs an UDP Scan.                                                                                |
| `-sV`                | Scans the discovered services for their versions.                                                    |
| `-sC`                | Perform a Script Scan with scripts that are categorized as "default".                                |
| `--script <script>`  | Performs a Script Scan by using the specified scripts.                                               |
| `-O`                 | Performs an OS Detection Scan to determine the OS of the target.                                     |
| `-A`                 | Performs OS Detection, Service Detection, and traceroute scans.                                      |
| `-D RND:5`           | Sets the number of random Decoys that will be used to scan the target.                               |
| `-e`                 | Specifies the network interface that is used for the scan.                                           |
| `-S 10.10.10.200`    | Specifies the source IP address for the scan.                                                        |
| `-g`                 | Specifies the source port for the scan.                                                              |
| `--dns-server <ns>`  | DNS resolution is performed by using a specified name server.                                        |
| -iL                  | Performs defined scans against targets in provided 'hosts.lst' list.                                 |
| -sT                  | uses the TCP three-way handshake to determine if a specific port on a target host is open or closed. |
| --script <category>  | Uses specified NSE scripts.                                                                          |
|                      |                                                                                                      |

## NSE scripts

|             |                                                                                                                                         |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `auth`      | Determination of authentication credentials.                                                                                            |
| `broadcast` | Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans. |
| `brute`     | Executes scripts that try to log in to the respective service by brute-forcing with credentials.                                        |
| `default`   | Default scripts executed by using the `-sC` option.                                                                                     |
| `discovery` | Evaluation of accessible services.                                                                                                      |
| `dos`       | These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services.              |
| `exploit`   | This category of scripts tries to exploit known vulnerabilities for the scanned port.                                                   |
| `external`  | Scripts that use external services for further processing.                                                                              |
| `fuzzer`    | This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time.     |
| `intrusive` | Intrusive scripts that could negatively affect the target system.                                                                       |
| `malware`   | Checks if some malware infects the target system.                                                                                       |
| `safe`      | Defensive scripts that do not perform intrusive and destructive access.                                                                 |
| `version`   | Extension for service detection.                                                                                                        |
| `vuln`      | Identification of specific vulnerabilities.                                                                                             |
## performance

|                              |                                                              |
| ---------------------------- | ------------------------------------------------------------ |
| `--max-retries <num>`        | Sets the number of retries for scans of specific ports.      |
| `--stats-every=5s`           | Displays scan's status every 5 seconds.                      |
| `-v/-vv`                     | Displays verbose output during the scan.                     |
| `--initial-rtt-timeout 50ms` | Sets the specified time value as initial RTT timeout.        |
| `--max-rtt-timeout 100ms`    | Sets the specified time value as maximum RTT timeout.        |
| `--min-rate 300`             | Sets the number of packets that will be sent simultaneously. |
| `-T <0-5>`                   | Specifies the specific timing template.                      |

## output options
|                |                                                                                   |
| -------------- | --------------------------------------------------------------------------------- |
| `-oA filename` | Stores the results in all available formats starting with the name of "filename". |
| `-oN filename` | Stores the results in normal format with the name "filename".                     |
| `-oG filename` | Stores the results in "grepable" format with the name of "filename".              |
| `-oX filename` | Stores the results in XML format with the name of "filename".                     |
|                |                                                                                   |
```
xsltproc target.xml -o target.html
```
convert xml file to html.
