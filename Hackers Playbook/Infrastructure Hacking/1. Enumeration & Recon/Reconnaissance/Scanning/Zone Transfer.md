Command to perform zone transfer:
```
dig @[DNS SERVER HERE] axfr [DOMAIN NAME HERE]
```
### DNS zone
Every website has a root server containing the primary domain name (.com). This primary domain can be broken down into different zones. With a DNS server in place, these subdomains will be assigned a unique name which can be based on things like locations or departments within an organization.

A company that operates all over the world will have different domains on the servers based on a particular location, e.g. company.co.uk (UK) and company.au (Australia). The domain in Australia may also have subdomains throughout different cities, but having all this on one server can slow it down. To counter this, domains and their subdomains are organized into different zones.

DNS zones are represented and organized hierarchically with the .com domain at the top. DNS zones may contain one domain, many domains, or many subdomains.

### DNS zone file
DNS records can hold a wealth of information. This file specifies all the information about a certain zone. Every DNS zone file must contain the name of the DNS zone and the TTL (time to live). TTL is used to specify how long records are kept in the DNS server cache. This file can also contain information about the network’s topology and DNS mapping (including IP addresses of other servers or potentially even email addresses of the server admins). Zone files also contain information about the DNS record type; they describe information about a specific object and the different types have different associations. For example, a type A record specifies an IPv4 address for a given host.

### Zone transfer
A zone transfer is a specific DNS request to retrieve all information on that DNS server’s zone. A misconfigured DNS server will give anyone access to the DNS records if they ask for it. This technique can be valuable for penetration testers as it can reveal all the subdomains and IP addresses, enabling them to predict the IP schema and identify different servers in the network. It is a useful tool for reconnaissance of a target, especially during the enumeration stage. Zone transfers use TCP (Transmission Control Protocol) but where this is disabled, it is often possible to brute force addresses by using a wordlist of host names and submitting many queries to the server.

![Example of a domain with multiple zones. Some of the domains include .com (which also has the subdomains .labs and .labs2), .net, and .gov. A subdomain of .edu (.beu) also has its own subdomain (.comp)](https://il-labforge-assets.origin.immersivelabs.team/uploads/erx9etrdDtOp7FiJDhhcR1QmhsFLPJdcDVftqxQFJ0A.png)