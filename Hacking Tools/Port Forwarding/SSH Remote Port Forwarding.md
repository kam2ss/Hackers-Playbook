### Remote Port Forwarding
The connection starts from the client (victim listening on the port 9090) and it goes to the intermediary SSH server and to the local ssh client (attacker who's listening on port 80).


**Client (9090) --> Remote SSH Server --> Local SSH Client (Attacker)**
```
ssh -R 9090:IP:80 USER@Machine_IP
```