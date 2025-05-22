### HTTP TRACE
TRACE is an HTTP method used to ***echo*** input data from a request. The TRACE method makes it easy to understand what's been sent as part of an HTTP request and what the server receives from that request by echoing it back.
It can be used to test, debug, or perform connection analysis, but has historically been used to perform cross-site tracing (XST) attacks that can bypass `httpOnly` flags set on cookies. 
This leads to ***authentication*** cookies being stolen and used to impersonate users.

It's recommended to set the `TraceEnable` directive in Apache servers to `Off`.
For example, in `/etc/apache2/conf-enabled/security.conf`:
```
# TraceEnable On
TraceEnable Off
```

Once TRACE is disabled, restart the service:
```
# In older version of apache2:
service apache2 restart

# For Debian
sudo systemctl restart apache2.service

# For RHEL, CentOS
sudo systemctl restart httpd.service
```

### Testing if TRACE is enabled
with the `curl` command:
```
# Even the url is 'https', only use 'http':
curl -v -X TRACE http://www.example.com
```