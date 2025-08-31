```bash
ffuf -u http://sub.target.com -H "Host: FUZZ.target.com" -w /usr/share/seclists/Discovery/DNS/namelist.txt -mc 200
```