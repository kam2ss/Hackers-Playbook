Rule-Based attacks are also known as `hybrid attacks.` It assumes the attacker knows something about the password policy.

It is an optimized password-cracking method where specific rules are applied to a dictionary wordlist to generate password variations.

#### How Rule-Based Attacks Work
Instead of just testing words from a wordlist (password, letmein, 123456), a rule-based attack ***modifies*** them based on common variations, such as:
- Adding numbers (password123)
- Replacing letters with symbols (p@ssw0rd)
- Capitalizing (password, PASSWORD)
- Appending years (hello2024)

#### John
John the ripper has a config file that contains rule sets, which is located at `/etc/john/john.conf` or `/opt/john/john.conf`.
```
cat /etc/john/john.conf|grep "List.Rules:" | cut -d"." -f3 | cut -d":" -f2 | cut -d"]" -f1 | awk NF
```

We have many rules that are available to use. Now create a `single-password.txt` file with the password `tryhackme`.
```
cat > single-password.txt
```

**best64**. This will expand out password list from 1 to 76 passwords.
```
john --wordlist=single-password.txt --rules=best64 --stdout | wc -l
```

**KoreLogic** uses various built-in and custom rules to generate complex password lists.
```
john --wordlist=single-password.txt --rules=KoreLogic --stdout | grep "Tryh@ckm3"
```

---
#### Custom Rules
We can build our own rule(s)  and use it at run time while john is cracking the hash or use the rule to build a custom wordlist.

We can add our rule to the end of `john.conf`. The format is `[symbols]word[0-9]`.
```
sudo vim /etc/john/john.conf
[List.Rules:THM-Password-Attacks]
Az"[0-9]" ^[!@#$]
```
- `[List.Rules:THM-Password-Attacks]:` specify the rule name THM-Password-Attacks.
- `Az:` represents a single word from the original wordlist/dictionary using -p.
- `"[0-9]":` append a single digit (from 0 to 9) to the end of the word. For two digits, we can add "[0-9][0-9]"  and so on.  
- `^[!@#$]:` add a special character at the beginning of each word. ^ means the beginning of the line/word. Note, changing ^ to $ will append the special characters to the end of the line/word.

Create a file containing a single word `password`:
```
echo "password" > single-pass.lst
```

Use the rule:
```
john --wordlist=single-pass.lst --rules=THM-Password-Attacks --stdout
```