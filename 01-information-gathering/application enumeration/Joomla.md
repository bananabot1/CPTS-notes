**Overview:**
- Joomla is a free and open-source CMS used for discussion forums, photo galleries, e-Commerce, user-based communities, and more. It is written in PHP and uses MySQL in the backend.  
- Joomla can be enhanced with over 7,000 extensions and over 1,000 templates.
- Joomla collects some anonymous  usage statistics such as the breakdown of Joomla, PHP and database versions and server operating systems in use on Joomla installations. This data can be queried via their public API.
---
```
url -s https://developer.joomla.org/stats/cms_version | python3 -m json.tool
```

`droopescan scan joomla --url http://<domainnameoripaddress>`
