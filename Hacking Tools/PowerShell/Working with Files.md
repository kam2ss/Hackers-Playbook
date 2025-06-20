### What is PowerShell?
Similar to using `cat` in linux, we can use the `Get-Content` cmdlet in PowerShell to read the contents of a file. When `Get-Content` is run, the contents of the file is read and the result can be stored in a variable for later use or displayed on screen.

When reading in a file with `Get-Content`, it is possible to specify how much of the file is read. This is similar to the `head` and `tail` commands in Linux.

With the `-TotalCount` parameter you can specify how many lines you would like PowerShell to read from the top, e.g. `Get-Content <PATH> -TotalCount 5`.

The `-Tail` parameter will do the same but from the bottom of the file.

### Writing content to a file - Set-Content
In addition to reading files it is possible to write data to them, either by using `Set-Content` to create and overwrite files or `Add-Content` which can append content to an existing file.
```powershell
Set-Content -Value "This is a test" -Path ./test.txt
```

