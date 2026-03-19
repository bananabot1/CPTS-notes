**Overview:**
- osTicket is an open-source support ticketing system. It can be compared to systems such as Jira, OTRS, Request Tracker, and Spiceworks. 
- osTicket can integrate user inquiries from email, phone, and web-based forms into a web interface. 
- osTicket is written in PHP and uses a MySQL backend. It can be installed on Windows or Linux.
---
## Discovery

osTicket does not expose itself with Nmap. It can be identified by:
- Cookie named `OSTSESSID` set on page visit
- Footer text: `powered by osTicket` or `Support Ticket System`
- EyeWitness screenshots during web discovery

---
## Information Gathering via Tickets

Submitting support tickets can be used to extract internal information through staff responses:

- **Email addresses**: staff reply addresses can be used against the admin panel or for OSINT
- **Usernames**: names and roles revealed in correspondence
- **Internal details:** staff may inadvertently disclose system names, software versions, or infrastructure details when attempting to reproduce reported issues.