##### Determine whether anonymous connection (null session) is allowed on the Samba or not
Anonymous collection is allowed since no errors are thrown while connecting to samba server without any credentials
```
# If Null login is allowed
rpcclient -U "" -N IP

# If password is obtained
rpcclient -U admin IP

# Get OS info
srvinfo
```

##### Find SID of user 
```
lookupnames admin
```

##### Find Domain Groups that exists on the Samba Server
```
enumdomgroups
```