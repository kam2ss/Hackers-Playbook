### AWK
```
awk 'BEGIN {system("/bin/bash")}'
```

### Find
```
find . -exec /bin/bash \; -quit
```

### Vim
```
vim -c ':!/bin/bash'
```