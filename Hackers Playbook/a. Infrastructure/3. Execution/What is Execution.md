The attacker executes code on the compromised system after getting initial foothold.

**Tactics Used:**
- Running scripts (PowerShell, Bash, Python)
- Dropping malware (RATs, keyloggers, ransomware)
- Fileless attacks (living-off-the-land techniques, `LOLBins`)

**Example:**
- The phishing document **executes a PowerShell script**, which downloads a **reverse shell** to connect to the attacker’s machine.