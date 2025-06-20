#### AD Environment
The following is a list of Active Directory components that we need to be familiar with:
**Domain Controllers**
A server that provides Active Directory services and controls the entire domain. It provides encryption of user data as well as controlling access to a network, including users, groups, policies, and computers. It also enables resource access and sharing.

**Organizational Units**
OU's are containers within the AD domain with a hierarchical structure.

**AD objects**
It can be a single user or a group, or a hardware component, such as a computer or printer. Each domain holds a database that contains object identity information that creates an AD environment, including:
- Users - A security principal that is allowed to authenticate to machines in the domain
- Computers - A special type of user accounts
- GPOs - Collections of policies that are applied to other AD objects

**AD Domains**
These are the collection of Microsoft components within an AD network.

**Forest**
A collection of domains that trust each others.

#### Users and Groups Management
**AD Service Accounts**
- `Built-in local users:` These accounts are used to ***manage the system locally***, which is not part of the AD environment. 
- `Domain users:` Accounts with access to an active directory environment can ***use the AD services*** (managed by AD). 
- `AD Managed service accounts:` These are limited domain user account with ***higher privileges to manage AD services***.
- `Domain Administrators:` User accounts that can manage information in an Active Directory environment, including AD configurations, users, groups, permissions, roles, services, etc.

The following are Active Directory Administrators accounts:

| Group                     | Permissions                                                |
| ------------------------- | ---------------------------------------------------------- |
| **BUILTIN\Administrator** | Local admin access on a domain controller                  |
| **Domain Admins**         | Administrative access to all resources in the domain       |
| **Enterprise Admins**     | Available only in the forest root                          |
| **Schema Admins**         | Capable of modifying domain/forest; useful for red teamers |
| **Server Operators**      | Can manage domain servers                                  |
| **Account Operators**     | Can manage users that are not in privileged groups         |

**LDAP Hierarchical Tree Structure**
- **CN (Common Name)**: This refers to the name of an object in the directory. It's commonly used to identify a specific object, like a user or a computer. For example, "CN=JohnDoe" could represent a user object named John Doe.
    
- **DN (Distinguished Name)**: This is the full, unique path to an object in Active Directory. It provides the complete identity of an object, including its position in the directory hierarchy. A DN consists of several components like CN, OU, DC, etc., and it uniquely identifies an object. For example, "CN=JohnDoe,OU=Users,DC=example,DC=com".
    
- **OU (Organizational Unit)**: This is a container in AD that can hold objects like users, groups, and computers. OUs are used to organize objects in a hierarchical manner within the domain. For example, "OU=Users" could be an OU that contains all user accounts.
    
- **DC (Domain Component)**: This represents a domain in the Active Directory hierarchy. Each part of the domain name (e.g., "example.com") is represented by a "DC" component. For example, "DC=example,DC=com" is the domain component for the "example.com" domain.

An example of a Distinguished Name (DN) combining these components could be:
`CN=JohnDoe,OU=Users,DC=example,DC=com`

