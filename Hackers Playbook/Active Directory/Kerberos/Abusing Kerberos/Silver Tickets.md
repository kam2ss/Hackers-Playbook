#### Silver Tickets
Silver Tickets are similar to Golden Tickets, but only grant access to a specific service account. With local admin access to a machine, an attacker can dump the service account hash (or the machine account hash for local identities) to forge a service ticket.

The requirements for creating a Silver Ticket:
- Target server's FQDN
- The Kerberos service running on the server
- NTLM hash of the service
	- Domain account hash when running under a domain user
	- Machine account hash when running under a local identity