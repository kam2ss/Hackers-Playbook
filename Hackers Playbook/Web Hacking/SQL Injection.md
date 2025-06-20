SQL injection is a type of injection attack that affects any databases that might be included in an application.

```
" or "1"="1

SELECT username FROM users WHERE username == "admin" AND password == "" or "1"="1"
```