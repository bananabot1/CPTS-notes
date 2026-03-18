**Overview:**
WordPress is an open-source Content Management System (CMS) that can be used for multiple purposes. It’s often used to host blogs and forums. 
It's customizability and extensible nature make it prone to vulnerabilities through third-party themes and plugins. 
WordPress is written in PHP and usually runs on Apache with MySQL as the backend.
WordPress stores its plugins in the `wp-content/plugins` directory. This folder is helpful to enumerate vulnerable plugins. Themes are stored in the `wp-content/themes` directory. These files should be carefully enumerated as they may lead to RCE.

---
```
curl -s http://blog.inlanefreight.local | grep WordPress

```
```
poi1111@htb[/htb]$ curl -s http://blog.inlanefreight.local/ | grep themes


```
```
curl -s http://blog.inlanefreight.local/ | grep plugins
```