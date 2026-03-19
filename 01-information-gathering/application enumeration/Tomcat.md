**Overview:**
- Apache Tomcat is an open-source web server that hosts applications written in Java. Tomcat was initially designed to run Java Servlets and Java Server Pages (JSP) scripts. However, its popularity increased in Java-based frameworks and is now widely used by frameworks such as Spring and tools such as Gradle.
- Tomcat is often less apt to be exposed to the internet. We see it from time to time on external pentests and can make for an excellent foothold into the internal network. I
- It is far more common to see Tomcat (and multiple instances, for that matter) during internal pentests. More often than not, at least one instance internal is configured with weak or default credentials.
---
Discovery
Tomcat servers can be identified by the Server header in the HTTP response. If the server is operating behind a reverse proxy, requesting an invalid page should reveal the server and version

default structure

        shellsession
`├── bin ├── conf │   ├── catalina.policy │   ├── catalina.properties │   ├── context.xml │   ├── tomcat-users.xml │   ├── tomcat-users.xsd │   └── web.xml ├── lib ├── logs ├── temp ├── webapps │   ├── manager │   │   ├── images │   │   ├── META-INF │   │   └── WEB-INF |   |       └── web.xml │   └── ROOT │       └── WEB-INF └── work     └── Catalina        └── localhost`
The `bin` folder stores scripts and binaries needed to start and run a Tomcat server. The `conf` folder stores various configuration files used by Tomcat. The `tomcat-users.xml` file stores user credentials and their assigned roles. The `lib` folder holds the various JAR files needed for the correct functioning of Tomcat. The `logs` and `temp` folders store temporary log files. The `webapps` folder is the default webroot of Tomcat and hosts all the applications. The `work` folder acts as a cache and is used to store data during runtime.
Each folder inside `webapps` is expected to have the following structure.

        shellsession
`webapps/customapp ├── images ├── index.jsp ├── META-INF │   └── context.xml ├── status.xsd └── WEB-INF     ├── jsp    |   └── admin.jsp    └── web.xml    └── lib    |    └── jdbc_drivers.jar    └── classes        └── AdminServlet.class`

The most important file among these is `WEB-INF/web.xml`, which is known as the deployment descriptor. This file stores information about the routes used by the application and the classes handling these routes

enumeration
```
 gobuster dir -u http://web01.inlanefreight.local:8180/ -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt 
```
e'll typically want to look for the `/manager` and the `/host-manager` pages. We can attempt to locate these with a tool such as `Gobuster` or just browse directly to them.