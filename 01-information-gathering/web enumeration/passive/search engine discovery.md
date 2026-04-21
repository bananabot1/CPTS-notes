**Overview:**
- Uses search engine indexes to gather information about a target without sending any requests to it.
- Search operators allow precise queries for exposed files, login pages, configuration files, credentials, and other sensitive data indexed publicly.
- Google Dorking (Google Hacking) combines operators to surface misconfigurations and unintentionally exposed resources. Extended dork lists available at the Google Hacking Database (GHDB).
---
## Search Operators

|Operator|Description|
|---|---|
|`site:`|Limit results to a specific domain|
|`inurl:`|Match term in the URL|
|`intitle:`|Match term in the page title|
|`intext:` / `inbody:`|Match term in page body text|
|`filetype:`|Filter by file extension|
|`cache:`|View cached version of a page|
|`link:`|Find pages linking to a URL|
|`related:`|Find sites similar to a URL|
|`numrange:`|Match numbers within a range|
|`allintext:`|All terms must appear in body|
|`allinurl:`|All terms must appear in URL|
|`allintitle:`|All terms must appear in title|
|`AND`|Require all terms|
|`OR`|Match any term|
|`NOT` / `-`|Exclude term or results|
|`*`|Wildcard for any word|
|`" "`|Exact phrase match|
|`..`|Numerical range search|

---
## Google Dorks

```
site:<target> inurl:login
site:<target> (inurl:login OR inurl:admin)
```

Login page discovery.

```
site:<target> filetype:pdf
site:<target> (filetype:xls OR filetype:docx)
```

Exposed file discovery.

```
site:<target> inurl:config.php
site:<target> (ext:conf OR ext:cnf)
```

Configuration file discovery.

```
site:<target> inurl:backup
site:<target> filetype:sql
```

Database backup discovery.