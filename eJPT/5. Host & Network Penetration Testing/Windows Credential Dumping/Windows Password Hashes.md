- The Windows OS stores hashed user account passwords locally in the ***SAM*** (***Security Accounts Manager***) database. 
- ***Authentication*** and ***verification*** of user credentials is facilitated by the ***Local Security Authority*** (LSA).
- Windows versions up to Windows Server 2003 utilize two different types of hashes:
	- **LM** (Now disabled from Windows Vista onwards)
	- **NTLM**

### SAM Database
- ***SAM is a database file*** that is responsible for ***managing user accounts and passwords*** on windows.
- `The SAM database file cannot be copied while the operating system is running.`
- The Windows NT ***kernel keeps the SAM database file locked*** and as a result attackers typically utilize in-memory techniques and tools to dump SAM hashes from the LSASS process.
- In modern version of Windows, the ***SAM database is encrypted*** with a `syskey`.

### LM 
- LM `LanMan` is the default hashing algorithm that was implemented in windows OS systems prior to NT4.0.
- The protocol is used to hash user passwords, and the hashing process can be broken down into the following steps:
	- The password is broken into two seven-character chunks.
	- All characters are then converted into uppercase.
	- Each chunk is then hashed separately with the DES algorithm.
- LM hashing is a weak protocol and does not include salts, consequently making brute-force and rainbow table attacks effective against LM hashes.

### NTLM (`NTHash`)
- NTLM is a collection of authentication protocols that are utilized in windows to facilitate authentication between computers.
- When a user is created, it is encrypted with the ***MD4*** hashing algorithm, while the original password is disposed of.
- NTLM improves upon LM in the following ways:
	- Does not split the hash in to two chunks
	- Case sensitive
	- Allows the use of symbols and Unicode characters.
- ***Does not use salt***.
