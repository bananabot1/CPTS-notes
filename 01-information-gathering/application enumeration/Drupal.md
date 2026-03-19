**Overview:**
- Drupal is an open-source CMS that is popular among companies and developers. 
- Drupal is written in PHP and supports using MySQL or PostgreSQL for the backend.
- Additionally, SQLite can be used if there's no DBMS installed. Like WordPress, Drupal allows users to enhance their websites through the use of themes and modules
-  Drupal indexes its content using nodes. A node can hold anything such as a blog post, poll, article, etc. The page URIs are usually of the form `/node/<nodeid>`.
---
Discovery
A Drupal website can be identified in several ways, including by the header or footer message `Powered by Drupal`, the standard Drupal logo, the presence of a `CHANGELOG.txt` file or `README.txt file`, via the page source, or clues in the robots.txt file such as references to `/node`
```
 curl -s http://drupal.inlanefreight.local | grep Drupal
```

enumeration
