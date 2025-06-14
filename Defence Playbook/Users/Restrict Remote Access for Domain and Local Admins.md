#### **Objective:**  
**Local Admin (built-in) and Domain Admin Accounts** can ***retain full privileges during a remote connection*** when performing Lateral Movement in the network, `because UAC does not apply to the built-in Administrator by default`, and `Domain Admins are not restricted by UAC remote restrictions`.

So, reduce lateral movement risk by limiting remote access capabilities of domain and local administrator accounts.

Note: `Built-in Administrator Account is disabled on Windows client systems, and enabled on Windows Servers by default.`

---
#### 1. **Restrict Local Admin Remote Access**
**Enable UAC Remote Restrictions:**
`[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System] "LocalAccountTokenFilterPolicy"=dword:00000000`
- Default value is `0` (enabled).
- Prevents local admins from having full tokens during remote access.

---
#### 2. **Restrict RDP Access to Admin Accounts**
**Group Policy Path:**
`Computer Configuration > Windows Settings > Security Settings > Local Policies > User Rights Assignment`
**Policy:**
- `Deny log on through Remote Desktop Services`
- Add:
    - `Domain Admins`
    - `Local Administrators` group

This prevents RDP access even if credentials are known.

---
#### 3. **Limit WinRM / WMI / PSExec Access**
**Remove admin groups from remote access control:**
- Disable WinRM if not needed:    
	 ```powershell
	Stop-Service WinRM Set-Service WinRM -StartupType Disabled
	```
    
- Restrict WMI namespace permissions via **WMI Control**:
    - Run `wmimgmt.msc` → Properties → Security
    - Remove/deny remote access for `Administrators`

---
#### 4. **Tiered Access Model**
- Implement **Admin Tiering** (e.g., Tier 0 for domain controllers).
- Ensure:
    - Domain Admins **only log into Tier 0 systems**.
    - No admin re-use across different privilege levels.

---
#### Summary

|Account Type|Remote Method|Defense|
|---|---|---|
|Local Admin|WMI/PSExec/WinRM|UAC Remote Restrictions|
|Domain Admin|RDP|Group Policy: Deny RDP|
|Both|General|Tiered Access + Remove Unneeded Services|
