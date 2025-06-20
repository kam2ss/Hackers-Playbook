#### Backup website files
* tar -czvf website_files.tar.gz /var/www/html

#### Backup MySQL database
* mysqldump -u username -p database_name > database_backup.sql

#### Backup Apache configuration
* tar -czvf apache_config.tar.gz /etc/apache2
