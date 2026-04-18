**Overview:**
The use of cloud, such as [AWS](https://aws.amazon.com/), [GCP](https://cloud.google.com/), [Azure](https://azure.microsoft.com/en-us/), and others, is now one of the essential components for many companies nowadays beacuse, all companies want to be able to do their work from anywhere, so they need a central point for all management. This is why services from `Amazon` (`AWS`), `Google` (`GCP`), and `Microsoft` (`Azure`) are ideal for this purpose.
The configurations made by the administrators may nevertheless make the company's cloud resources vulnerable. This often starts with the `S3 buckets` (AWS), `blobs` (Azure), `cloud storage` (GCP), which can be accessed without authentication if configured incorrectly.

#### Company Hosted Servers

        shellsession
`poi1111@htb[/htb]$ for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done blog.inlanefreight.com 10.129.24.93 inlanefreight.com 10.129.27.33 matomo.inlanefreight.com 10.129.127.22 www.inlanefreight.com 10.129.127.33 s3-website-us-west-2.amazonaws.com 10.129.95.250`

Often cloud storage is added to the DNS list when used for administrative purposes by other employees. This step makes it much easier for the employees to reach and manage them. Let us stay with the case that a company has contracted us, and during the IP lookup, we have already seen that one IP address belongs to the `s3-website-us-west-2.amazonaws.com` server.

However, there are many different ways to find such cloud storage. One of the easiest and most used is Google search combined with Google Dorks. For example, we can use the Google Dorks `inurl:` and `intext:` to narrow our search to specific terms. In the following example, we see red censored areas containing the company name.