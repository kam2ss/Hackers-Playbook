##### Find the Server Description of Samba
```
smbclient -L IP -N
```

##### List all available shares on the Samba
```
# If Null login is allowed
smbclient -L IP -N

# If password is obtained
smbclient -L IP -U admin
```

##### Log in to the Share
```
smbclient //IP/public -N
smbclient //IP/jane -U jane
```