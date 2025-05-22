It allows attackers to ***extract sensitive authentication data such as passwords, tokens, and hashes*** directly from a system's memory. These extracted credentials can be used such as pass-the-hash, pass-the-ticket, or even generating Golden Kerberos tickets. 

### Some Use Cases
**Credential extraction**
Using this module, attackers can ***steal plaintext passwords, hashes, and Kerberos tickets*** for all logged-in users.
```
sekurlsa::logonpasswords
```

**Pass-the-hash (PTH) attacks**
Once the NTLM hash is acquired, this module can be used to ***authenticate to other systems without needing the plaintext passwords***.
```
sekurlsa::pth
```

**Golden and Silver tickers**
Attackers can forge Kerberos tickets to maintain persistence within a network. 

### Mimikatz Modules
Mimikatz is structured around different modules, each serving a specific function in extracting or manipulating data from a windows system.

The Mimikatz module and command structure are as follows:
```
module::command
```

**Commonly Used Modules**
- **sekurlsa**: Security Local Security Authority, ***interacts with the system's memory to extract credentials***, such as cleartext passwords, hashes, and Kerberos tickets. For example: `sekurlsa::logonpasswords` to retrieve password information for logged-in users.
- **Kerberos**: This module is used to ***manipulate Kerberos tickets***, which are essential for authentication in Windows networks. Attackers can use commands like `kerberos::golden` to create golden tickets or `kerberos::list` to display available Kerberos tickets.
- **lsadump**: This module ***extracts sensitive information, such as credentials and security policies, from the Local Security Authority (LSA)***. A common command is `lsadump::sam` for dumping password hashes from the Security Account Manager (SAM).
- **privilege**: This module is crucial for elevating permissions within Mimikatz. Running `privilege::debug` grants the tool the necessary privileges to access restricted memory areas and perform many of its core functions.

### Usage
**Privilege debug module**
Mimikatz requires elevated permissions to interact with sensitive system memory and extract credential information.

On Windows, certain operations (like reading memory to extract credentials) require debug privileges. Running this command is crucial because it elevates the tool's permissions to enable many of its core functionalities.
```
privilege::debug
```

