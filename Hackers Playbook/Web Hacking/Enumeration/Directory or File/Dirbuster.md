Its purpose is to find common pages within a web application.

### Case Sensitive Target
Choosing a wordlist requires some thought. Firstly, we need to find ***if the target is case sensitive***? This can be done by simple `curl` request on the command line:

**Curl**
```
curl http://web-application/login
```

If this request to the target returns the ***same response for both*** `/login` and `/Login`, you can assume ***it's case insensitive***. For web applications that are case insensitive or only ever utilize lowercase names, you should use a `directory-list-lowercase` wordlist.