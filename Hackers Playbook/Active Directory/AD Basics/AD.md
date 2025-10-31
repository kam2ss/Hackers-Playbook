#### What is Active Directory (AD)?
Microsoft ***database and services*** used to ***manage permissions and access*** to a network.

#### Why use AD?
Handles ***authentication/authorization***. It is used to ***centralize and simplify Identity and Access Management*** in Windows-based environments.

#### AD Objects
- Users
- Groups
- Shared folders
- Computers
- GPO: collection of policy settings

#### Active Directory Domain Services (ADDS)
- Stores and organises information about ***users, computers, groups,*** and other networks in a hierarchical database.
- Any server running AD DS is a Domain Controller (DC).
- Every DC holds a complete copy of domain database.
	- AD Replication

#### Kerberos 

#### AD Structure
**Example**
- `Forest:` Alphabet
- `Tree:` Squarespace, YouTube
- `Domain:` Regional Office
- `Organizational Unit (OU):` HR/IT/Finance

#### Windows AD with Linux/Other?
- With some config, Linux machines can be domain joined
- ***Enabled by packages like `realmd` using `sssd`***
- File shares for lateral movement from Linux/Windows boxes (***SAMBA***)