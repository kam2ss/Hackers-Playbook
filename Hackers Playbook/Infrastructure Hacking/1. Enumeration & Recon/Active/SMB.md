#### Connect to the Files share of IP 192.168.1.1 under username **user**.
```
smbclient \\\\192.168.1.1\\Files -U user
smbclient //192.168.1.1/Files -U user
```

#### To list all the shares
```
smbclient -L <IP> -U user
```

### netexec
```
netexec smb IP -u USERNAME -p PASSWORD --shares
```