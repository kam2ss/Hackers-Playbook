#### Check the status of BIND
```
systemctl status bind9
```

#### Restart the BIND service
```
sudo systemctl restart bind9
```

#### Reload BIND configuration
```
sudo rndc reload
```

#### Check the BIND zone file syntax
```
# named-checkzone <zone_name> <zone_file>
named-checkzone secrets.com /etc/bind/db.secrets.com
```

