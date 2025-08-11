```bash
gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt -t 50 -x php,txt -s 200,204,301,302 -o gobuster.txt
```