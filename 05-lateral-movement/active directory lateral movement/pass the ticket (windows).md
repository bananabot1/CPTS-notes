**Overview:**
A method for moving laterally in an Active Directory environment is called a [Pass the Ticket (PtT) attack](https://attack.mitre.org/techniques/T1550/003/). In this attack, we use a stolen Kerberos ticket to move laterally instead of an NTLM password hash. 
The Kerberos authentication system is ticket-based. The central idea behind Kerberos is not to give an account password to every service you use. Instead, Kerberos keeps all tickets on your local system and presents each service only the specific ticket for that service, preventing a ticket from being used for another purpose.

- The `Ticket Granting Ticket` (`TGT`) is the first ticket obtained on a Kerberos system. The TGT permits the client to obtain additional Kerberos tickets or `TGS`.
- The `Ticket Granting Service` (`TGS`) is requested by users who want to use a service. These tickets allow services to verify the user's identity.

When a user requests a `TGT`, they must authenticate to the domain controller by encrypting the current timestamp with their password hash. Once the domain controller validates the user's identity (because the domain knows the user's password hash, meaning it can decrypt the timestamp), it sends the user a TGT for future requests. Once the user has their ticket, they do not have to prove who they are with their password.
## Pass the Ticket (PtT) attack

We need a valid Kerberos ticket to perform a `Pass the Ticket (PtT)` attack. It can be:

- Service Ticket (TGS) to allow access to a particular resource.
- Ticket Granting Ticket (TGT), which we use to request service tickets to access any resource the user has privileges.

Before we perform a `Pass the Ticket (PtT)` attack, let's see some methods to get a ticket using `Mimikatz` and `Rubeus`.

## Scenario

Let's imagine we are on a pentest, and we manage to phish a user and gain access to the user's computer. We found a way to obtain administrative privileges on this computer and are working with local administrator rights. Let's explore several ways we can manage to get access tickets on this computer and how we can create new tickets.

## Harvesting Kerberos tickets from Windows

On Windows, tickets are processed and stored by the LSASS (Local Security Authority Subsystem Service) process. Therefore, to get a ticket from a Windows system, you must communicate with LSASS and request it. As a non-administrative user, you can only get your tickets, but as a local administrator, you can collect everything.

We can harvest all tickets from a system using the `Mimikatz` module `sekurlsa::tickets /export`. The result is a list of files with the extension `.kirbi`, which contain the tickets.