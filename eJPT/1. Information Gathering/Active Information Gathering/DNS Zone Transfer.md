A DNS Zone Transfer, also sometimes known by the including DNS query type ***AXFR***, is a type of DNS transaction. It is the  process in the DNS that ***duplicates DNS records*** from the Master DNS zone to the Backup DNS zone. This process allows you to replicate your DNS records on various alternative name servers. 

It uses the Transaction Control Protocol (TCP) for zone transfer and takes the form of a client-server transaction. `DNS zone transfer does not provide authentication.`

Tools:
`dnsenum` is a comprehensive tool and perform multiple actions:
```
dnsenum zonetransfer.me
```


`dig` can be used to perform zone transfer, but first we need NS names. To get that, we need to use tool `nslookup` and `dig +short`.
nslookup will only show one soa:
```
nslookup -type=soa zonetransfer.me
```

but dig +short will show multiple soa:
```
dig +short NS zonetransfer.me
```

Once, we get the NS names we can use dig tool:
```
dig axfr @nsztm1.digi.ninja zonetransfer.me
```


Using fierce tool, however it doesn't perform zone transfer:
```
fierce -dns zonetransfer.me
```