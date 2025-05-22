##### Attacker
```
echo SANS > file.txt
```

##### Victim's Machine
```
nc -l -p 2222 > received2.txt
```

##### Attacker
```
nc ip 2222 < file.txt
```

##### Victim's Machine
```
type received.txt
```

The text should be 'SANS'