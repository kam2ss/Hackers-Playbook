#### Two types of Administrator Accounts relevant to lateral movement:
- `Local administrators:` part of the Local Administrators group
- `Domain administrators:` part of the Local Administrators group

The differences we are interested in are ***restrictions imposed by `User Account Control (UAC)` over local administrators*** (except for the ***default Administrator account***). 
- Only ***allowing*** full privileges during interactive ***RDP sessions***.
- ***Blocking*** remote administrative tasks requested via ***RPC, SMB, or WinRM*** with a filtered, ***limited token*** preventing the account from doing privileged actions.
- The ***`default Administrator account will get the full privileges`***.

***Domain accounts*** with local admin rights ***do not face these UAC restrictions*** and have full admin privileges ***remotely***.

#### Interactive Logon (via `RDP`)
- When a Domain Admin Logs in via RDP, UAC does not stirp privileges - they get full admin rights.

#### Remote Admin Access (via `WMI`, `PSEexec`, `WinRM`)
If you're using a domain admin account for lateral movement via non-interactive methods, and the target has the UAC restrictions enabled:
- Local Admin account will lose the privileges
- Domain Admin account will keep the privileges.

#### What is a default Administrator Account
The **default Administrator account** refers to the ***built-in local user*** named **"Administrator"** on a Windows machine.

- It’s created automatically when Windows is installed.
- It has the **highest privileges** on the local machine.
- Unlike other local admin accounts, it **is not subject to UAC restrictions remotely** — it always gets full administrative rights.

This account is often disabled by default for security reasons but can be enabled by an admin.

