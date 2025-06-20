#### Wapiti
Wapiti is a web scanner that adds fuzzing to its toolkit. It can ***send payloads to the target applications***.
```
wapiti -u http://web-application -m all
```

- By default, Wapiti runs a subset of the modules included with the tool. However, for a more comprehensive scan, the `-m` flag allows the user to specify which modules to run.