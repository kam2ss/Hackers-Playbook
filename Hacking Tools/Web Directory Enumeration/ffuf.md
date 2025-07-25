#### `ffuf` (Fuzz Faster U Fool) is a fast web fuzzing tool to ***brute force or discover hidden files, directories, parameters, subdomains, and more on web servers.

#### Common Use Cases
**Parameter discovery (GET or POST)
```bash
ffuf -u https://target.com/page.php?FUZZ=value -w params.txt
```

**Login fuzzing (with POST)
```bash
ffuf -u http://<target>.com/login -X POST -d "user=admin&pass=FUZZ" -w /usr/share/wordlists/rockyou.txt
```

**Directory brute-forcing (discover hidden paths)**
```bash
ffuf -u https://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

**Virtual host fuzzing**
What is it?
- Virtual hosts ***allow one web server (one IP) to host multiple websites/domains*** by using the ***Host header*** in HTTP requests.

How it works?
- You send HTTP requests to the **same IP address** but change the **Host header** each time.
- The placeholder `FUZZ` is replaced by entries from a wordlist (common subdomains or hostnames).
```bash
ffuf -u http://target.com -H "Host: FUZZ.target.com" -w subdomains.txt
```

#### Key options
- `-u` → Target URL with `FUZZ` as placeholder
- `-w` → Wordlist
- `-X` → HTTP method (GET/POST)
- `-H` → Custom header (e.g. for Host fuzzing or tokens)
- `-d` → POST data
- `-fc` → Filter by status code (e.g. `-fc 404`)
- `-mc` → Match only specific status code (e.g. `-mc 200`)