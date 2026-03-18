**Overview:**
WordPress is an open-source Content Management System (CMS) that can be used for multiple purposes. It’s often used to host blogs and forums. 
It's customizability and extensible nature make it prone to vulnerabilities through third-party themes and plugins. 
WordPress is written in PHP and usually runs on Apache with MySQL as the backend.
WordPress stores its plugins in the `wp-content/plugins` directory. This folder is helpful to enumerate vulnerable plugins. Themes are stored in the `wp-content/themes` directory. These files should be carefully enumerated as they may lead to RCE.

---
Discovery/footprinting

        shellsession
`User-agent: * Disallow: /wp-admin/ Allow: /wp-admin/admin-ajax.php Disallow: /wp-content/uploads/wpforms/ Sitemap: https://inlanefreight.local/wp-sitemap.xml`
browsing to the `/robots.txt` file.
Here the presence of the `/wp-admin` and `/wp-content` directories would be a dead giveaway that we are dealing with WordPress.

Enumeration
```
curl -s http://blog.inlanefreight.local | grep WordPress

```
```
poi1111@htb[/htb]$ curl -s http://blog.inlanefreight.local/ | grep themes


```
```
curl -s http://blog.inlanefreight.local/ | grep plugins
```
grepping for the `wp-content` directory, `themes` and `plugin`, and begin building a list of interesting data points.
we can go further and attempt to fingerprint the theme version number and look for any known vulnerabilities that affect them

WPScan
`sudo wpscan --url <http://domainnameoripaddress> --enumerate`
Runs wpscan using the `--enmuerate` flag. Can replace the url with any valid and reachable URL in each challenge

`sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url <http://domainnameoripaddress>`
Runs wpscan and uses it to perform a password attack (`--password-attack`) against the specified url and references a word list (`/usr/share/wordlists/rockyou.txt`)