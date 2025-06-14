#### What is Kerberos?
**Kerberos** is a secure ***network authentication protocol*** that ***operates over non-secure networks*** using a ***ticket-based system***. It is primarily designed for a ***client-server model***, where ***both parties can verify each other's identity*** - a process known as ***mutual authentication.***
- It uses ***symmetric-key cryptography,*** which ***ensures confidentiality and integrity***.
- All authentication relies on a ***trusted third party*** called the ***Key Distribution Centre (KDC)***.
- Clients first request a ***Ticket Granting Ticket (TGT)*** from the KDC, then use it to obtain ***service tickets*** for accessing network resources securely.

#### Security Feature
Kerberos helps protect against ***eavesdropping*** and ***replay attacks*** by ***encrypting*** messages and including ***timestamps*** in protocol exchanges.

#### How Kerberos authentication works on Windows networks
1. User sends ***username + timestamp*** (encrypted using a key derived from the password) to the ***Key Distribution Centre (KDC).***