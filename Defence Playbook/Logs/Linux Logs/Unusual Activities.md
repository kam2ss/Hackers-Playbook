## Multiple failed login attempts:
#### Failed SSH Logins
* grep 'Failed password' /var/log/auth.log

#### Successful SSH Logins
* grep 'Accepted password' /var/log/auth.log

#### Sudo Usage
* grep 'COMMAND=' /var/log/auth.log


## Requests for pages that don't exist:
#### Access Logs
* grep '404' /var/log/apache2/access.log
* grep '404' /var/log/nginx/access.log