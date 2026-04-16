**Overview:**
- ICMP tunneling encapsulates traffic within `ICMP packets` containing `echo requests` and `responses`. ICMP tunneling would only work when ping responses are permitted within a firewalled network. 
- When a host within a firewalled network is allowed to ping an external server, it can encapsulate its traffic within the ping echo request and send it to an external server. The external server can validate this traffic and send an appropriate response, which is extremely useful for data exfiltration and creating pivot tunnels to an external server.
----
```
git clone https://github.com/utoni/ptunnel-ng.git
```

Once the ptunnel-ng repo is cloned to our attack host, we can run the `autogen.sh` script located at the root of the ptunnel-ng directory.

#### Building Ptunnel-ng with Autogen.sh

        shellsession
`poi1111@htb[/htb]$ sudo ./autogen.sh`

After running autogen.sh, ptunnel-ng can be used from the client and server-side