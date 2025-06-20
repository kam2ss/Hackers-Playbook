**Searches files by names, doesn't search inside a file**
```
Get-ChildItem -Path C:\ -Recurse -Filter FILENAME
```
Note: `Filter` only works on `file name`, not full path.

****