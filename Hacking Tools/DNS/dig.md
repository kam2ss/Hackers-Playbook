### Queries
#### Basic DNS Query
```
dig <domain>
```

#### Query a specific DNS server
```
# dig @<DNS_Server_ip> <domain>
dig @10.10.10.10 www.secrets.com
```

#### Show detailed output for all records
```
dig +all <domain>
```

#### Get specific types of DNS records
**A record:**
```
dig <domain> A
```

**MX record:**
```
dig <domain> MX
```

**NS record:**
```
dig <domain> NS
```

**TXT record:**
```
dig <domain> TXT
```

**SOA record:**
```
dig <domain> SOA
```

**All records (ANY):**
```
dig <domain> ANY
```


### Attacks
#### Zone Transfer 
```
dig -t AXFR DOMAIN_NAME @DOMAIN_IP
```

