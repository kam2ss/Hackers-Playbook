#### What is a RID?
When a user is created, an identifier called `Relative ID (RID)` is assigned to them. The ***RID is simply a numeric identifier representing the user*** across the system. 

**In Windows:**
- `500:` assigned to the default **Administrator** account.
- `1000 and above:` assigned to the regular users.

#### How Windows uses RID?
When a **user logs on**:
- The ***LSASS process gets its RID from the SAM registry hive*** and ***creates an access token*** associated with that RID. 
- This token determines what the user can and cannot do on the system.

### Exploiting RID to Gain Admin Access
If we can ***modify the SAM registry*** value and give a normal user the same **RID (500)** as the Administrator:
- Windows will assign that unprivileged user an **Administrator-level access token**.
- This allows the user to **gain full administrative privileges** without being a member of the Administrative group.

---

**Find the assigned RIDs for any user**
```cmd
wmic useraccount get name,sid
```

#### Assign the RID to a user
To do so, we need to ***access the SAM using regedit***. The ***SAM is restricted to the SYSTEM account only***, so even the Administrator won't be able to edit it.

1. **To run the `Regedit` as SYSTEM**
```cmd
PsExec64.exe -i -s regedit
```

This will bring the `Regedit` GUI windows as SYSTEM.

2. **Navigate in Regedit**
`HKLM\SAM\SAM\Domains\Account\Users\`

3. **Find the User's key**
	- Convert its RID (e.g. `1010` = `0x3F2`) to hex.

4. Edit the `F` value inside the kye:
	- Locate the RID at offset `0x30`.
	- Replace it with **Administrator's RID (`500` = `0X01f4` -> little-endian = `F401`).

The next time the `<user>` logs in, LSASS will associate it with the same RID as Administrator and grant them the same privileges.
