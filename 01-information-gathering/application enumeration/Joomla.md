**Overview:**
- Joomla is a free and open-source CMS used for discussion forums, photo galleries, e-Commerce, user-based communities, and more. It is written in PHP and uses MySQL in the backend.  
- Joomla can be enhanced with over 7,000 extensions and over 1,000 templates.
- Joomla collects some anonymous  usage statistics such as the breakdown of Joomla, PHP and database versions and server operating systems in use on Joomla installations. This data can be queried via their public API.
---
```
url -s https://developer.joomla.org/stats/cms_version | python3 -m json.tool
```

`droopescan scan joomla --url http://<domainnameoripaddress>`
Runs `droopescan` against a joomla site located at the specified url
```
sudo python3 joomla-brute.py -u http://dev.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```
Runs joomla-brute.py tool with python3 against a specified url, utilizing a specified wordlist (`/usr/share/metasploit-framework/data/wordlists/http_default_pass.txt`) and user or list of usernames (`-usr`)