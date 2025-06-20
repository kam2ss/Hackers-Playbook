A brute-force attack is a password-cracking method where an ***attacker systematically tries all possible combinations of characters until the correct password is found***. This method is time-consuming but effective if the password is weak or short.

#### Hashcat
`hashcat` has charset options that could be used to generate your own combinations.
```
hashcat --help
```

We can use `hashcat` with the brute-force attack mode with a combination of our choice.
```
hashcat -a 3 ?d?d?d?d --stdout
```
- `-a 3:` sets the attack mode as a brute-force 
- `?d?d?d?d:` tells hashcat to use four digits.
- `--stdout:` print the result to the terminal.