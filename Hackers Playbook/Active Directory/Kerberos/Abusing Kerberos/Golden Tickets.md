#### Golden Tickets
In AD Kerberos authentication, the client doesn't directly authenticate to the server; ***authentication relies on the trust between the server and the KDC***. A valid TGT presented to the KDC is accepted automatically without a question. 

A Golden Ticket can be created by anyone with:
- The FQDN of the domain
- The SID of the domain
- The username to impersonate
- The KRBTGT account's password hash - (requires high domain privileges)

With domain access, the first three points are trivial to extract. The fourth one requires high domain privileges since this account can only be found on domain controllers.

Using this info, tools like Metasploit can generate a TGT valid for any user, allowing persistent access even if it breaks normal policies.