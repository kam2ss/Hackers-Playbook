Logs from your applications can assist in debugging and monitoring. 

### Apache
Apache has built-in functionality for logging `access, errors, and custom` logs.

### Error Log
The server error is defined by the `ErrorLog` directive. The error log is where Apache HTTPd will send diagnostic information and process errors encountered in processing requests.

The `ErrorLog` directive defines where the log is stored while it's being recorded. It's followed by one parameter as the *output directory* in the format `directory/filename`.
```
ErrorLog /va/log/apache2/error.log
```
To implement an alternative error log, change the configuration file to any other directory and filename by using `ErrorLog {directory}/{filename}`:
```
ErrorLog {log_output_directory}/error.log
```

#### Default error log:
```
cat /var/log/apache2/error.log

# If the error log is not present:
touch /var/log/apache2/error.log
```

### Access Log
The **server access** log is used to record all requests being sent to the server. These logs can be reformatted by using `LogFormat` directive. Various pre-existing log formats, which can be found within the `apache2.conf` file.

### Custom Access Log
To add a custom access logger, add a line in the `apache2.conf` file with the `CustomLog` directive and specify the directory with the filename and the custom log format to be used.
```
CustomLog {log_output_directory}/custom_access_log.log combined
```

### Why is logging important?
***Incident Response***
Incident response strategies rely on the presence of rich, detailed logs.

### Apache Environment Variables
Environment variables are set in Apache to control operations and communicate with other programs such as CGI scripts. These environment variables can be found in the Apache configuration folder within the `envvars` file.

#### LogLevel
It's a directive that can be configured from:
```
vim /etc/apache2/apache2.conf

# Search for LogLevel
```

For question 9, create a access.log in `/var/log/apache2/` and also edit the `apache2.conf` file:
```
# Create a file called access.log:
touch /var/log/apache2/access.log

# Modify the file with the path variable to the access.log in apache2.conf:
vim /etc/apache2/apache2.conf

# Input the below line under the LogFormat
CustomLog ${APACHE_LOG_DIR}/access.log combined
```