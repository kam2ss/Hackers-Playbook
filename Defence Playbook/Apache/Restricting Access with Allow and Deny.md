### Allow and Deny directives
When dealing with Apache 2.2 or earlier versions, the `Allow` and `Deny` directives are used to control who is able to access certain locations of the webserver.

### Allow from and Deny from
Both directives must declare from what host they are allowing or denying. For e.g.
```
# This will deny access to everyone. 
Deny from all

# Allow access from only with the domain or sub-domain of 'immersivelabe.local'
# Hosts can also be partial or full ip addresses
Allow from immersivelabs.local
Allow from 10.102 or 10.102.8.35
```

### Order directive
`Order` is an aptly named directive that dictates in which order `Allow` or `Deny` commands are applied:
	`deny,allow`: Evaluate deny directives before allow
	`allow,deny`: Evaluate allow directives before deny
	`mutual-failure`: If a host appears on an allow list and not on a deny list, they are allowed access.

```
# Only allowing access from 'immersivelabs.local' because the 'deny' is applied first:
Order deny,allow
allow from immersivelabs.local
deny from all
```

### FilesMatch
```
# Limiting access based on certain file extensions with 'FilesMatch':
<Directory /var/www/html/music/>
  AllowOverride None
  Order deny,allow
  Deny from all
  <FilesMatch "\.(mp3|aac|flac)$">
    Allow from all
  </FilesMatch>
</Directory>
```

### Restart Apache
```
# In older version of apache2:
service apache2 restart

# For Debian
sudo systemctl restart apache2.service

# For RHEL, CentOS
sudo systemctl restart httpd.service
```