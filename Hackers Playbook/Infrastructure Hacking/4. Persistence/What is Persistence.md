The attacker ensures continued access even after reboots or security updates.

**Tactics Used:**
- Registry modifications (Windows Run keys)
- Creating new user accounts (backdoor admin accounts)
- Scheduled tasks & services (hidden scripts running on startup)
- DLL hijacking (injecting malware into system processes)

**Example:**
- The attacker creates a **hidden admin user** (`net user backdoor /add`) so they can log in later even if the malware is removed.