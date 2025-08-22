##### Get OS Information
```
enum4linux -o IP
```

##### List all Users that exist on the Samba Server
```
# If Null login is allowed
enum4linux -U IP -N

# If password is obtained
enum4linux -u admin -p password -S IP
```

##### List all available shares on the Samba Server
```
enum4linux -S IP
```

##### Find Domain Groups that exists on the Samba Server
```
enum4linux -G IP
```

##### Find Printer Information
```
enum4linux -i IP
```

##### List SID of Unix Users by performing RID Cycling
```
enum4linux -u admin -p password1 -r IP
```