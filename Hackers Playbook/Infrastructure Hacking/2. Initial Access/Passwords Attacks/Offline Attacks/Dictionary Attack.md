A dictionary attack is a technique used to ***guess passwords by using well-known words or phrases***. It ***relies entirely on pre-gathered wordlists*** that were previously generated or found.

We need to identify the type of hash first, [[Identify the Hash]].

#### Hashcat
```
hashcat -a 0 -m 0 HASH /usr/share/wordlists/rockyou.txt
```
- `-a 0:` sets the attack mode to a dictionary attack
- `-m 0:` sets the hash mode for cracking MD5 hashes

```
hashcat --show
```