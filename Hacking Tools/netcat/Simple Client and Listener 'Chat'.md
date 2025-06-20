##### Listener
```
nc -l -p 2222
```
##### Attacker
```
nc ip 2222
```

#### Pull a File
```
echo this is text > text.txt
nc -l -p 2222 < text.txt
```
