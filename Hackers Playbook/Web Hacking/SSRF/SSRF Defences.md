There are usually two approaches to this:

**Deny List**
A Deny List ***allows all requests except those specified or matching certain patterns***. A Web Application may employ a deny list to restrict access to sensitive endpoints, IP addresses, or domains, such as localhost or 127.0.0.1. However, attackers can bypass the Deny List using alternative localhost references such as 0, 0.0.0.0, 0000 or subdomains that have a DNS record pointing to restricted IPs. 

In cloud environments, blocking access to the IP 169.254.169.254, which holds sensitive server metadata, is recommended. Although, attackers can bypass it by using subdomains with DNS records pointing to that IP.

**Allow List**
An allow list denies all requests unless they appear on a list or match a particular pattern, such as https://website.thm. Attackers can bypass this by creating a subdomain, like https://website.thm.attackers-domain.thm, which would be allowed by the application.

**Open Redirect**
If the above bypasses do not work, there is one more trick, the open redirect. An attacker can exploit an open redirect, where an endpoint automatically redirects visitors to another website. For example, a link like [https://website.thm/link?url=https://tryhackme.com](https://website.thm/link?url=https://tryhackme.com) is used for tracking clicks. In the case of a stringent SSRF vulnerability allowing only URLs starting with [https://website.thm/](https://website.thm/), an attacker could use the open redirect to manipulate the internal HTTP request and redirect it to a domain of their choice.