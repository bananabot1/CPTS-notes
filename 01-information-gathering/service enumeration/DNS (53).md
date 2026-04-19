**Overview:**
`Domain Name System` (`DNS`) is an integral part of the Internet.
DNS is a system for resolving computer names into IP addresses, and it does not have a central database. Globally distributed DNS servers translate domain names into IP addresses and thus control which server a user can reach via a particular domain. There are several types of DNS servers that are used worldwide:

|**Server Type**|**Description**|
|---|---|
|`DNS Root Server`|The root servers of the DNS are responsible for the top-level domains (`TLD`). As the last instance, they are only requested if the name server does not respond. Thus, a root server is a central interface between users and content on the Internet, as it links domain and IP address. The [Internet Corporation for Assigned Names and Numbers](https://www.icann.org/) (`ICANN`) coordinates the work of the root name servers. There are `13` such root servers around the globe.|
|`Authoritative Nameserver`|Authoritative name servers hold authority for a particular zone. They only answer queries from their area of responsibility, and their information is binding. If an authoritative name server cannot answer a client's query, the root name server takes over at that point. Based on the country, company, etc., authoritative nameservers provide answers to recursive DNS nameservers, assisting in finding the specific web server(s).|
|`Non-authoritative Nameserver`|Non-authoritative name servers are not responsible for a particular DNS zone. Instead, they collect information on specific DNS zones themselves, which is done using recursive or iterative DNS querying.|
|`Caching DNS Server`|Caching DNS servers cache information from other name servers for a specified period. The authoritative name server determines the duration of this storage.|
|`Forwarding Server`|Forwarding servers perform only one function: they forward DNS queries to another DNS server.|
|`Resolver`|Resolvers are not authoritative DNS servers but perform name resolution locally in the computer or router.|

DNS is mainly unencrypted. Devices on the local WLAN and Internet providers can therefore hack in and spy on DNS queries. Since this poses a privacy risk, there are now some solutions for DNS encryption. By default, IT security professionals apply `DNS over TLS` (`DoT`) or `DNS over HTTPS` (`DoH`) here.
DNS also stores and outputs additional information about the services associated with a domain. A DNS query can therefore also be used, for example, to determine which computer serves as the e-mail server for the domain in question or what the domain's name servers are called.

|**DNS Record**|**Description**|
|---|---|
|`A`|Returns an IPv4 address of the requested domain as a result.|
|`AAAA`|Returns an IPv6 address of the requested domain.|
|`MX`|Returns the responsible mail servers as a result.|
|`NS`|Returns the DNS servers (nameservers) of the domain.|
|`TXT`|This record can contain various information. The all-rounder can be used, e.g., to validate the Google Search Console or validate SSL certificates. In addition, SPF and DMARC entries are set to validate mail traffic and protect it from spam.|
|`CNAME`|This record serves as an alias for another domain name. If you want the domain [www.hackthebox.eu](http://www.hackthebox.eu) to point to the same IP as hackthebox.eu, you would create an A record for hackthebox.eu and a CNAME record for [www.hackthebox.eu](http://www.hackthebox.eu).|
|`PTR`|The PTR record works the other way around (reverse lookup). It converts IP addresses into valid domain names.|
|`SOA`|Provides information about the corresponding DNS zone and email address of the administrative contact.|

All DNS servers work with three different types of configuration files:

1. local DNS configuration files
2. zone files
3. reverse name resolution files
The DNS server [Bind9](https://www.isc.org/bind/) is very often used on Linux-based distributions. Its local configuration file (`named.conf`) is roughly divided into two sections, firstly the options section for general settings and secondly the zone entries for the individual domains. The local configuration files are usually:

- `named.conf.local`
- `named.conf.options`
- `named.conf.log`

It contains the associated RFC where we can customize the server to our needs and our domain structure with the individual zones for different domains. The configuration file `named.conf` is divided into several options that control the behavior of the name server. A distinction is made between `global options` and `zone options`.

Global options are general and affect all zones. A zone option only affects the zone to which it is assigned. Options not listed in named.conf have default values. If an option is both global and zone-specific, then the zone option takes precedence.
A `zone file` is a text file that describes a DNS zone with the BIND file format. In other words it is a point of delegation in the DNS tree. The BIND file format is the industry-preferred zone file format and is now well established in DNS server software. A zone file describes a zone completely. There must be precisely one `SOA` record and at least one `NS` record. The SOA resource record is usually located at the beginning of a zone file. The main goal of these global rules is to improve the readability of zone files. A syntax error usually results in the entire zone file being considered unusable. The name server behaves similarly as if this zone did not exist. It responds to DNS queries with a `SERVFAIL` error message.

In short, here, all `forward records` are entered according to the BIND format. This allows the DNS server to identify which domain, hostname, and role the IP addresses belong to
For the `Fully Qualified Domain Name` (`FQDN`) to be resolved from the IP address, the DNS server must have a reverse lookup file. In this file, the computer name (`FQDN`) is assigned to the last octet of an IP address, which corresponds to the respective host, using a PTR record. The PTR records are responsible for the reverse translation of IP addresses into names

---

## Dangerous Settings
|                   |                                                                                |
| ----------------- | ------------------------------------------------------------------------------ |
| `allow-query`     | Defines which hosts are allowed to send requests to the DNS server.            |
| `allow-recursion` | Defines which hosts are allowed to send recursive requests to the DNS server.  |
| `allow-transfer`  | Defines which hosts are allowed to receive zone transfers from the DNS server. |
| `zone-statistics` | Collects statistical data of zones.                                            |
|                   |                                                                                |

## Enumeration

```
dig ns inlanefreight.htb @10.129.14.128
```
NS request to the specific nameserver. (explain in a line what ns is )

`dig any <domain.tld> @<nameserver>`
ANY request to the specific nameserver. (explain in one line what any is)

`dig axfr <domain.tld> @<nameserver>`
AXFR request to the specific nameserver.|(explain in a line what axfr is)

`dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>`
Subdomain brute forcing.