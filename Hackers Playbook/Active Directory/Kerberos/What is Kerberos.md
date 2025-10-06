#### The Kerberos Protocol
**Kerberos is a network security protocol** that works on the basis of ***tickets*** for authentication. It relies on ***Symmetric Key Cryptography*** and a ***trusted third party (KDC)*** to authenticate service requests between two or more hosts on a network, in a manner that provides resiliency to ***Machine-in-the-Middle (MitM) attacks.*** 

**The protocol requires three components to function:**
- Client
- Server
- Key Distribution Centre (KDC) - ***created when a Windows server is promoted to the DC role within a domain.***

**The KDC consists of two servers:**
- Authentication Server (AS) - `creates TGT (Ticket Granting Ticket)`
- Ticket-Granting Service (TGS) - `creates the Service Ticket`

#### Kerberos Tickets
The Kerberos protocol uses **Tickets** to ***securely transmit encrypted keys (called session keys)***, which ***authenticate users to domain-joined computers and services***, and ***set limits on the timeframe*** (time-bound and only works for a limited period) that such authentication is valid.

**Two different types of tickets are used in the process:**
- Ticket-Granting Ticket (TGT) - also known as a primary ticket
- Service Ticket (TGS)

#### Initial Authentication
**Purpose:** Prove the ***user is legitimate*** and get a ***Ticket Granting Ticket (TGT).***

When a ***user successfully authenticates to a domain-joined workstation***, their ***password is hashed and used as a key to encrypt a timestamp*** , which is sent to the ***DC*** as an ***authentication server request (AS-REQ).*** `Note: Including encrypted timestamp ins the AS-REQ prevents replay attacks (an attacker can't reuse an old authentication message)`

The ***DC verifies it by decrypting the request with the user's password hash in AD***, and ***confirms the timestamp is within acceptable time limits*** (one of the reasons that accurate system time is required across a Windows domain). The DC then ***responds with an authentication server reply (AS-REP).***

The ***AS-REP contains:
- `the TGT:` ***which is encrypted with the `krbtgt` user's password hash***
- `user's logon session key:` which is ***encrypted using the authenticating user's password hash***.

#### Service Authentication
**Purpose:** Use the ***TGT*** to get ***Service Tickets*** for specific resources (file server, SQL, etc).

Now that the user is authenticated to the domain (you have a TGT), you must still request a ***Service Ticket (TGS)*** from the DC each time you want to use a service - even services on your own computer.

This is achieved via a ***Ticket-Granting Service Request (TGS-REQ).*** The TGS-REQ is similar in structure to the AS-REQ but also contains:
- The name of the requested service - (`HOST/<computer name>` for local services)
- The Ticket-Granting Ticket (TGT)
- The Client Session Key

The ***DC uses the TGT to verify the authenticity of the request*** and then returns a Ticket-Granting Service Request (TGS-REP), which contains:
- `TGS with (PAC=your group membership/privileges):` encrypted with the requested service password hash
- `Service Session Key:` encrypted with the client session key

Even local authentication uses Kerberos, the requested name would comprise `HOST/<computer name>`, and is the ***Service Principal Name*** used for built-in Windows services. 

#### Service Principal Name
The ***`HOST` portion is known as a Service Principal Name (SPN).*** An SPN is a unique identifier in Kerberos that ***maps a service to an account***. When an SPN is set for an account, the KDC generates a ***service key*** for that account, which is used to encrypt the service ticket. By decrypting the ticket, the server proves its identity to the client. Accounts with SPNs in AD are called ***service accounts.***


