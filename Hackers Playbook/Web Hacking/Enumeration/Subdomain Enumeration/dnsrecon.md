Bruteforce DNS enumeration tries different possible subdomains from a pre-defined list of commonly used subdomains.

#### Basic Enum
```bash
dnsrecon -d <target.com>
```
- `-d:` specifies the domain to target

#### DNS Recon Tool
```
dnsrecon -t brt -d acmeitsupport.thm
```
- `-t:` type of enumeration to perform
- `brt:` perform bruteforce

#### Brute-Force with a wordlist
```bash
dnsrecon -d <target.com> -t brt -D /usr/share/seclists/Discover/DNS/subdomains-top1million-11000.txt -j subs.json
```

