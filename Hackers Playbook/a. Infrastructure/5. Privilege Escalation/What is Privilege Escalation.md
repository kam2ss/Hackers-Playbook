The attacker elevates privileges to gain full control over the system.

**Tactics Used:**
- Exploiting kernel vulnerabilities (`DirtyPipe`, `PrintNightmare`)
- Credential dumping (Mimikatz, LSASS dump)
- Token impersonation (stealing admin tokens)

**Example:**
- The attacker runs **Mimikatz** to steal local **admin credentials** and use them to execute commands as SYSTEM.