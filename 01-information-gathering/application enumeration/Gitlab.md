**Overview:**
- GitLab is a web-based Git-repository hosting tool that provides wiki capabilities, issue tracking, and continuous integration and deployment pipeline functionality. 
- During internal and external penetration tests, it is common to come across interesting data in a company's GitHub repo or a self-hosted GitLab or BitBucket instance. These Git repositories may just hold publicly available code such as scripts to interact with an API. 
- Applications such as GitLab allow for public repositories (that require no authentication), internal repositories (available to authenticated users), and private repositories (restricted to specific users). It is also worth perusing any public repositories for sensitive data and, if the application allows, register an account and look to see if any interesting internal repositories are accessible.
---
## Enumeration
The first thing we should try is browsing to `/explore` and see if there are any public projects that may contain something interesting.
Public projects can be interesting because we may be able to use them to find out more about the company's infrastructure, find production code that we can find a bug in after a code review, hard-coded credentials, a script or configuration file containing credentials, or other secrets such as an SSH private key or API key.