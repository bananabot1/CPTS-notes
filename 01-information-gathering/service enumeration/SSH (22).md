**Overview:**
[Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (`SSH`) enables two computers to establish an encrypted and direct connection within a possibly insecure network on the standard port `TCP 22`. An advantage of SSH is that the protocol runs on all common operating systems. Since it is originally a Unix application, it is also implemented natively on all Linux distributions and MacOS. SSH can also be used on Windows, provided we install an appropriate program. The well-known [OpenBSD SSH](https://www.openssh.com/) (`OpenSSH`) server on Linux distributions is an open-source fork of the original and commercial `SSH` server from SSH Communication Security. Accordingly, there are two competing protocols: `SSH-1` and `SSH-2`.

`SSH-2`, also known as SSH version 2, is a more advanced protocol than SSH version 1 in encryption, speed, stability, and security. For example, `SSH-1` is vulnerable to `MITM` attacks, whereas SSH-2 is not.

We can imagine that we want to manage a remote host. This can be done via the command line or GUI. Besides, we can also use the SSH protocol to send commands to the desired system, transfer files, or do port forwarding. Therefore, we need to connect to it using the SSH protocol and authenticate ourselves to it. In total, OpenSSH has six different authentication methods:

1. Password authentication
2. Public-key authentication
3. Host-based authentication
4. Keyboard authentication
5. Challenge-response authentication
6. GSSAPI authentication
#### Public Key Authentication

In a first step, the SSH server and client authenticate themselves to each other. The server sends its `public host key` to the client, which the client uses to verify the server's identity. Only when contact is first established there is a risk of a third party interposing itself between the two participants and thus intercepting the connection. A `host key` cannot be imitated because it is a unique public-private `key pair`, and an attacker cannot forge the private key’s signature without access to it, assuming the client properly verifies the public key against a trusted source.

After server authentication, however, the client must also prove to the server that it has access authorization. However, the SSH server is already in possession of the encrypted hash value of the password set for the desired user. As a result, users have to enter the password every time they log on to another server during the same session. For this reason, an alternative option for client-side authentication is the use of a public key and private key pair.

The private key is created individually for the user's own computer and secured with a passphrase that should be longer than a typical password. The private key is stored exclusively on our own computer and always remains secret. If we want to establish an SSH connection, we first enter the passphrase and thus open access to the private key.

Public keys are also stored on the server. The server creates a cryptographic problem with the client's public key and sends it to the client. The client, in turn, decrypts the problem with its own private key, sends back the solution, and thus informs the server that it may establish a legitimate connection. During a session, users only need to enter the passphrase once to connect to any number of servers. At the end of the session, users log out of their local machines, ensuring that no third party who gains physical access to the local machine can connect to the server.

----
## dangerous settings
|**Setting**|**Description**|
|---|---|
|`PasswordAuthentication yes`|Allows password-based authentication.|
|`PermitEmptyPasswords yes`|Allows the use of empty passwords.|
|`PermitRootLogin yes`|Allows to log in as the root user.|
|`Protocol 1`|Uses an outdated version of encryption.|
|`X11Forwarding yes`|Allows X11 forwarding for GUI applications.|
|`AllowTcpForwarding yes`|Allows forwarding of TCP ports.|
|`PermitTunnel`|Allows tunneling.|
|`DebianBanner yes`|Displays a specific banner when logging in.|

## enumeration
```
./ssh-audit.py 10.129.14.132
```
checks the client-side and server-side configuration and shows some general information and which encryption algorithms are still used by the client and server

```
```
ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password
```
```
if the ssh server allows it, changes the auth method. In this case, a password authentication method co