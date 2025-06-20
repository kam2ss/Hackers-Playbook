Running services on a webserver as a root user can be useful when binding ports to startup services. However, it comes with certain risks such as:
	Traceability: When commands are run as `sudo`, they are automatically logged.
	Increased risk of attack: If a vulnerability exists that could be exploited.

### Apache
Traditionally it's required to run services with a `root` account to bind ports below 1024.
Apache2 utilizes the `www-data` user to run services.

To check if your services are running as a non-root user, navigate to the configuration file; the directive for `user` and `group` are provided there:
```
export APACHE_RUN_USER=rootexport APACHE_RUN_GROUP=root
```
If the directives for both user and group are set to `root`, you will need to change them to `www-data`.
If you can't find it with the above command, type it manually:
```
# To check the user and group:
sudo nano /etc/apache2/apache2.conf

# To make the changes from the root to www-data user and group:
sudo nano /etc/apache2/envvars
```

Once the directive has been changed, restart the Apache service:
```
# In older version of apache2:
service apache2 restart

# For Debian
sudo systemctl restart apache2.service

# For RHEL, CentOS
sudo systemctl restart httpd.service
```

