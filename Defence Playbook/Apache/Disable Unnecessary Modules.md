Modules are programs that can provide various purposes such as logging, encrypting, authenticating, and even database management.

### Apache
The `mod_info` module gives information on the webserver configuration of a web page.
```
# To find out which modules are currently enabled on your server:
apache2ctl -M

# Helper functions to enable or disable modules in Apache:
a2dismod #disable module
a2enmod #enable module

# To disable the 'mod_headers' module:
a2dismod mod_headers
```

### Restart Apache:
```
# In older version of apache2:
service apache2 restart

# For Debian
sudo systemctl restart apache2.service

# For RHEL, CentOS
sudo systemctl restart httpd.service
```