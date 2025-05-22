To continuously display the output of the `ps aux`:
```
watch -d -n 0.1 "ps aux"
```
- `watch:` runs the given command repeatedly 
- `-d:` highlights any differences in the output between consecutive runs of the command
- `-n 0.1:` sets the interval between updates to **0.1 seconds**, meaning the command will run every 0.1 seconds
- 