**Overview:**
- Jenkins is an open-source automation server written in Java, running in servlet containers such as Tomcat. Used for continuous integration and automated testing.
- Frequently found on internal networks running as the SYSTEM account on Windows servers, making it a high-value target for AD foothold.
- Known for weak or default credentials and instances with authentication disabled entirely.
---
## Discovery

Jenkins runs on port `8080` by default. Port `5000` is used for master/slave communication. Fingerprint by the login page at `/login` or the security configuration page at `/configureSecurity/`.

---
## Default Credentials

Try `admin:admin` or attempt access with no credentials. Unauthenticated Jenkins instances are not uncommon on internal networks.
