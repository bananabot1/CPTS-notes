**Overview:*
- Cloud storage (S3 buckets on AWS, blobs on Azure, cloud storage on GCP) is a common misconfiguration target. When access controls are not explicitly set, these resources may be publicly accessible without authentication.
- Cloud storage endpoints are often added to company DNS for internal management convenience, making them discoverable through standard subdomain enumeration and DNS analysis.
- Google Dorks (`inurl:`, `intext:`) are an effective first pass for finding exposed cloud resources indexed by search engines.
- domain.glass provides infrastructure insight on a target domain, including cloud provider relationships and associated assets.
- GrayHatWarfare indexes publicly exposed cloud storage across AWS, Azure, and GCP. Supports filtering by file format, making it useful for finding sensitive files left accessible in misconfigured buckets or blobs.
---
## Google Dorks

|Dork|Purpose|
|---|---|
|`inurl:<company> s3.amazonaws.com`|Find exposed AWS S3 buckets|
|`inurl:<company> blob.core.windows.net`|Find exposed Azure blobs|
|`inurl:<company> storage.googleapis.com`|Find exposed GCP storage|
|`intext:<company> site:s3.amazonaws.com`|Search indexed S3 content|

---
## Third-Party Resources

|Resource|Purpose|
|---|---|
|domain.glass|Infrastructure and cloud provider mapping for a target domain|
|GrayHatWarfare|Index of publicly exposed AWS, Azure, and GCP storage, filterable by file type|

- Cloud storage (S3 buckets on AWS, blobs on Azure, cloud storage on GCP) is a common misconfiguration target. When access controls are not explicitly set, these resources may be publicly accessible without authentication
- Cloud storage endpoints are often added to company DNS for internal management convenience, making them discoverable through standard subdomain enumeration and DNS analysis
- Google Dorks (`inurl:`, `intext:`) are an effective first pass for finding exposed cloud resources indexed by search engines. Combining target-specific terms with storage-related keywords narrows results quickly
- domain.glass provides infrastructure insight on a target domain, including cloud provider relationships and associated assets
- GrayHatWarfare indexes publicly exposed cloud storage across AWS, Azure, and GCP. Supports filtering by file format, making it useful for finding sensitive files left accessible in misconfigured buckets or blobs

---
## Google Dorks

|Dork|Purpose|
|---|---|
|`inurl:<company> s3.amazonaws.com`|Find exposed AWS S3 buckets|
|`inurl:<company> blob.core.windows.net`|Find exposed Azure blobs|
|`inurl:<company> storage.googleapis.com`|Find exposed GCP storage|
|`intext:<company> site:s3.amazonaws.com`|Search indexed S3 content|

---
## Third-Party Resources

|Resource|Purpose|
|---|---|
|domain.glass|Infrastructure and cloud provider mapping for a target domain|
|GrayHatWarfare|Index of publicly exposed AWS, Azure, and GCP storage, filterable by file type|