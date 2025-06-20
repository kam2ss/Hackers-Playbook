#### The model of Relay Attack requirement
- Attacker
- Target
- Pivot

##### Attacker
Netcat's zero I/O mode (-z) with a 3 sec timeout (-w3)
```
nc -vvv -z -w3 Target-ip 80
```

##### Pivot
```
nc -vvv -z -w3 Target-ip 80
curl hxxp://target-ip
```

```
mkfifo namedpipe
ls -l namedpipe
nc -l -p 8080 < namedpipe | nc target-ip 80 > namedpipe
```

##### Attacker
```
curl hxxp://target-ip:8080
```