**Overview:**
Our tasks to accomplish for this section are:

- Enumerate the internal network, identifying hosts, critical services, and potential avenues for a foothold.
- This can include active and passive measures to identify users, hosts, and vulnerabilities we may be able to take advantage of to further our access.
- Document any findings we come across for later use. Extremely important!

We will start from our Linux attack host without domain user credentials. It's a common thing to start a pentest off in this manner. Many organizations will wish to see what you can do from a blind perspective, such as this, before providing you with further information for the test. It gives a more realistic look at what potential avenues an adversary would have to use to infiltrate the domain. It can help them see what an attacker could do if they gain unauthorized access via the internet (i.e., a phishing attack), physical access to the building, wireless access from outside (if the wireless network touches the AD environment), or even a rogue employee. Depending on the success of this phase, the customer may provide us with access to a domain-joined host or a set of credentials for the network to expedite testing and allow us to cover as much ground as possible.

start with `passive` identification of any hosts in the network, followed by `active` validation of the results to find out more about each host (what services are running, names, potential vulnerabilities, etc.). Once we know what hosts exist, we can proceed with probing those hosts, looking for any interesting data we can glean from them.

#### Key Data Points

|**Data Point**|**Description**|
|---|---|
|`AD Users`|We are trying to enumerate valid user accounts we can target for password spraying.|
|`AD Joined Computers`|Key Computers include Domain Controllers, file servers, SQL servers, web servers, Exchange mail servers, database servers, etc.|
|`Key Services`|Kerberos, NetBIOS, LDAP, DNS|
|`Vulnerable Hosts and Services`|Anything that can be a quick win. ( a.k.a an easy host to exploit and gain a foothold)|

---
## Host Identification
```
sudo tcpdump -i ens224
```
see what hosts and types of network traffic we can capture. This is particularly helpful if the assessment approach is "black box." We notice some [ARP](https://en.wikipedia.org/wiki/Address_Resolution_Protocol) requests and replies, [MDNS](https://en.wikipedia.org/wiki/Multicast_DNS), and other basic [layer two](https://www.juniper.net/documentation/us/en/software/junos/multicast-l2/topics/topic-map/layer-2-understanding.html) packets (since we are on a switched network, we are limited to the current broadcast domain) some of which we can see below. This is a great start that gives us a few bits of information about the customer's network setup.

---
## Network Poisoning

```
sudo responder -I <network-interface>
```

Captures LLMNR, NBT-NS, and MDNS queries on the interface.

---
## Host Discovery

```
fping -asgq 172.16.5.0/23
```

Ping sweep. `fping` with a few flags: `a` to show targets that are alive, `s` to print stats at the end of the scan, `g` to generate a target list from the CIDR network, and `q` to not show per-target results.

## Host enumeration
```
sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum
```
Now that we have a list of active hosts within our network, we can enumerate those hosts further. We are looking to determine what services each host is running, identify critical hosts such as `Domain Controllers` and `web servers`, and identify potentially vulnerable hosts to probe later (explain flags)

---
## Kerberos Enumeration

```
kerbrute userenum -d <domain> --dc <dc> <wordlist> -o <out-file>
```

Brute-forces valid usernames against a Kerberos domain controller. `-d` domain, `--dc` DC IP, `-o` output.
