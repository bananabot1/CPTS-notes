**Overview:**
- GitLab is a web-based Git-repository hosting tool that provides wiki capabilities, issue tracking, and continuous integration and deployment pipeline functionality. 
- During internal and external penetration tests, it is common to come across interesting data in a company's GitHub repo or a self-hosted GitLab or BitBucket instance. These Git repositories may just hold publicly available code such as scripts to interact with an API. 
- Applications such as GitLab allow for public repositories (that require no authentication), internal repositories (available to authenticated users), and private repositories (restricted to specific users). It is also worth perusing any public repositories for sensitive data and, if the application allows, register an account and look to see if any interesting internal repositories are accessible.
---
## Discovery

Identified by the GitLab logo on the login page. Version only confirmed via `/help` when logged in. Known vulnerable versions: `12.9.0`, `11.4.7`, `13.10.3`, `13.9.3`, `13.10.2`. 

---
## Enumeration

Browse `/explore` for public projects without authentication. If open registration is allowed, register and check again. Public projects can be interesting because they can provide more information about the company's infrastructure, like production code with a bug , hard-coded credentials, a script or configuration file containing credentials, or other secrets such as an SSH private key or API key.

---
## User & Email Enumeration

The registration form at `/users/sign_up` leaks valid usernames and emails even when sign-up is disabled. Use findings for credential reuse or password spraying.