Having a good wordlist is critical to carrying out a successful password attack.


#### Default Passwords
- [hxxps://cirt.net/passwords](https://cirt.net/passwords)
- [hxxps://default-password.info/](https://default-password.info/)
- [hxxps://datarecovery.com/rd/default-passwords/](https://datarecovery.com/rd/default-passwords/)

---
#### Weak Passwords
- [hxxps://www.skullsecurity.org/wiki/Passwords](https://www.skullsecurity.org/wiki/Passwords) - This includes the most well-known collections of passwords.
- [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords) - A huge collection of all kinds of lists, not only for password cracking.

---
#### Leaked Passwords
- [SecLists/Passwords/Leaked-Databases](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Leaked-Databases)

---
#### Combined Wordlists
Combine multiple wordlists into one large file:
```
cat file1.txt file2.txt file3.txt > combined_list.txt
```

To remove the duplicated words:
```
sort combined_list.txt | uniq -u > cleaned_combined_list.txt
```

---
#### Customized Wordlists
Tools such as `Cewl` can be used to effectively crawl a website and extract strings or keywords.
```
cewl -w list.txt -d 5 -m 5 hxxp://thm.labs
```
- `-w:` write contents to a file.
- `-m 5:` gathers strings that are 5 characters or more
- `-d 5:` is the depth level of web crawling/spidering (default 2)

---
#### Username Wordlists
We can generate username lists from the target's website. There is a tool `username_generator` that could help create a list with most of the ***possible combinations if we have a first name and last name***.
```
git clone hxxps://github.com/therodri2/username_generator.git
```

Generate the possible combinations:
```
echo "John Smith" > user.lst
python3 username_generator.py -w user.lst
```

---
#### Crunch
In this technique, we specify a range of characters, numbers, and symbols in our wordlist. `Crunch` is one of the many powerful tools for creating an offline wordlist.
```
crunch 2 2 01234abcd -o crunch.txt
```

If part of the password is known, and we know it starts with `pass` and follows two numbers, we can use the `%` symbol.
```
crunch 6 6 -t pass%%
```

---
#### CUPP - Common User Passwords Profiler
It is an automatic and interactive tool written in Python for creating custom wordlists. For instance, if you know some details about a specific target, such as their ***birthdate***, ***pet name***, ***company name***, etc. There's also support for a `1337/leet mode`, which substitutes the letters `a, i, e, t, o, s, g, z` with numbers.
```
git clone https://github.com/Mebus/cupp.git
```

Interactive mode:
```
python3 cupp.py -i
```

It also provides default usernames and passwords from the `Alteco` database
```
python3 cupp.py -a
```