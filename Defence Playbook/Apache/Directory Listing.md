Directory listing allows people to view the files and folders on a web server in a specific paths.

### Apache
When directory listing is enabled in Apache, it will also reveal some information about the server and its configuration such as the OS.

Apache2 has a wide range of modules that can be used in order to customise the way it serves content. There are helper functions available that you can use to quickly enable; `a2enmod` or `a2dismod`:
```
# Running this command in CLI:
a2dismod --force autoindex
```
The `autoindex` module is disabled with the above command, which is responsible for automatic directory listing.

Restart Apache2 service
```
# In older version of apache2:
service apache2 restart

# For Debian
sudo systemctl restart apache2.service

# For RHEL, CentOS
sudo systemctl restart httpd.service
```

For question 9, remove the `backup` folder from `/var/www/html/`