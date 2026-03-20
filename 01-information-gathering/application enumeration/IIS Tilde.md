**Overview:**
IIS tilde directory enumeration is a technique utilised to uncover hidden files, directories, and short file names (aka the `8.3 format`) on some versions of Microsoft Internet Information Services (IIS) web servers. This method takes advantage of a specific vulnerability in IIS, resulting from how it manages short file names within its directories.

When a file or folder is created on an IIS server, Windows generates a short file name in the `8.3 format`, consisting of eight characters for the file name, a period, and three characters for the extension. Intriguingly, these short file names can grant access to their corresponding files and folders, even if they were meant to be hidden or inaccessible.

The tilde (`~`) character, followed by a sequence number, signifies a short file name in a URL. Hence, if someone determines a file or folder's short file name, they can exploit the tilde character and the short file name in the URL to access sensitive data or hidden resources.

IIS tilde directory enumeration primarily involves sending HTTP requests to the server with distinct character combinations in the URL to identify valid short file names. Once a valid short file name is detected, this information can be utilised to access the relevant resource or further enumerate the directory structure.

---
#### Tilde Enumeration using IIS ShortName Scanner
```
java -jar iis_shortname_scanner.jar 0 5 http://10.129.204.231/
```

Fortunately, there is a tool called `IIS-ShortName-Scanner` that can automate the task of sending each letter of the alphabet in a HTTP request.  To use `IIS-ShortName-Scanner`, you will need to install Oracle Java on either Pwnbox or your local VM.