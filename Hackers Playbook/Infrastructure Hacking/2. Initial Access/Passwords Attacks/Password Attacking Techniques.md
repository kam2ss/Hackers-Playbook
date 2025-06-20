#### Online Vs Offline
- **Online:** These attacks occur in ***real-time against a live system***, such as a website, SSH service, or email server. The attacker attempts to authenticate by ***guessing passwords*** or using ***credential stuffing***.
	**Password Guessing**
		It is a method of ***guessing passwords for online protocols and services based on dictionaries***. Therefore, it's time considered time-consuming and opens up the opportunity to generate logs for the failed login attempts.
		
	**Example of Online Attack Tools:**
		`hydra -l admin -P password.txt ssh://IP`


- **Offline:** These attacks are performed against ***password hashes*** that have been extracted from a system. The attacker cracks the hash using ***dictionary, brute-force, or rule-based*** methods.
	**Password Cracking**
		It is a technique used for discovering passwords ***from encrypted or hashed data to plaintext data***. It is performed locally or on systems controlled by the attacker.

	**Examples of Offline Attack Tools:**
	Hashcat - High speed **GPU-based**
		`hashcat -m 1000 hash.txt rockyou.txt --force`

	John the Ripper - **CPU-based**
		`john --wordlist=rockyou.txt hash.txt`
	




