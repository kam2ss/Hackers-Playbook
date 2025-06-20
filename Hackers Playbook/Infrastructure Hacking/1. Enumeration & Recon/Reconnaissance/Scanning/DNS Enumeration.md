### DNS zones
Authoritative DNS servers contain one or more DNS zones. Each zone can have its own configurable rules and DNS information, ranging from CNAME to TXT records. Often large businesses will run several DNS servers across their network estate for reasons of resilience and availability. The use of DNS zone transfers allows a simple way to update all of these servers from a single managed authoritative DNS server.

When an update to an authoritative zone occurs, other domain servers which also use this zone will want to replicate the changes. Instead of updating each server individually, DNS servers can perform regular zone transfer to update their DNS records. When a secondary DNS server initialises a zone transfer, it will request a copy of all zone entries and update its own records.

Misconfigured DNS servers may allow attackers to perform a zone transfer which will present all DNS records to the attacker. This might allow the attacker to gain knowledge of sensitive domains such as admin, or external services such as VPNs and webmail, or testing/UAT environments that may expose previously unknown attack surfaces.

### Securing DNS servers
Depending on the DNS server running, it is often possible to configure an IP whitelisting. This allows only trusted IP addresses to perform zone transfers, removing the ability to enumerate domain names.

Using BIND DNS, this can be done by modifying the *named.conf* configuration file using the ACL (access control list) option, which can then be added to a given zone.
```bash
acl bartertowngroup-trusted-zones {
 10.15.0.254; //ns2
 10.16.0.254; //ns3
};
zone bartertowngroup.com {
 type master;
 file "zones/bartertowngroup.com";
 allow-transfer { bartertowngroup-trusted-zones; };
};
```

The bartertowngroup.com server allows zone transfers to occur from either of the IP addresses found in the 'bartertowngroup-trusted-zones' ACL.
Where possible, admins can also implement Secret Key Transaction Authentication for DNS (TSIG) to secure communication between DNS servers. 

### DNS zone transfer
When initialising a zone transfer, the attacker will first need to know the name of the zone which they are targeting and then specify the IP address of the DNS server to perform the zone transfer against.

Use `dig` to launch a zone transfer against a domain with the following command:

```bash
dig domainName @DNS-Server-IP-Address axfr
```

