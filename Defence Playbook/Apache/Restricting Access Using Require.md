### Require Directive
When dealing with Apache 2.4 or later, the `Require` directive controls access to certain locations of the webserver.

### Require
It provides several ways to restrict access to resources based on specified restrictions through built-in authorization providers.
```
# This will deny access to everyone.
vim /etc/apache2/apache2.conf
<Directory /var/www/html/folder>
Require all denied
</Directory>

# Allows access from connections only with the domain or sub-domain
Require host immersivelabs.local

# Hosts can also be partial or full ipaddresses:
Require ip 10.102
Require ip 10.102.8.36
```

### RequireAny, RequireAll, and RequireNone directives
When we want to deny access only to a specific domain and grant access for everyone else.

#### RequireAny
- One or more enclosed `Require` directives must succeed for the `RequireAny` directive to succeed.
- This directive is the implied container when a `Require` directive is not enclosed.
#### RequireAll
- At least one `Require` directive must succeed, with no other directives failing, in order for the `RequireAll` directive to succeed.
#### RequireNone
- The enclosed `Require` directives must not succeed in order for the `RequireNone` directive to succeed.

##### Example:
```
I only want to allow people connecting from `immersivelabs.local` to have access:
<RequireAll>
  Require host immersivelabs.local
  Require all denied
</RequireAll>

# We have blocked access to everyone because `RequireAll` must not have any directive fail:
<RequireAny>
  Require host immersivelabs.local
  Require all denied
</RequireAny>
```

Limiting access based on certain ***file extensions*** with `Filematch`:
```
<Directory /var/www/html/music/>
  Require all denied
  <FilesMatch "\.(mp3|aac|flac)$">
    Require all granted
  </FilesMatch>
</Directory>
```

Limiting access based on certain ***ip address*** with `Filematch`:
```
<Directory /var/www/html/music/>
  Require all denied
  <FilesMatch "\.(mp3|aac|flac)$">
    Require ip 192.168
  </FilesMatch>
</Directory>
```
#### Restart Apache:
```
# In older version of apache2:
service apache2 restart

# For Debian
sudo systemctl restart apache2.service

# For RHEL, CentOS
sudo systemctl restart httpd.service
```
