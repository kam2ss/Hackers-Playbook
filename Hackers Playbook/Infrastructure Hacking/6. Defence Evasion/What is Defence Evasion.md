The attacker hide their activities to avoid detection.

**Tactics Used:**
- Disabling security tools (killing Windows Defender, bypassing EDR)
- Clearing logs (using `wevtutil` or `logman`)
- Obfuscating payloads (Base64 encoding, encryption)

**Example:**
- The attacker runs `taskkill /F /IM defender.exe` to disable **Windows Defender** and avoid detection.
