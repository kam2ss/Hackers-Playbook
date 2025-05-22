**-C 10 equivalent to Print 10 lines Above and Below**
```
Get-Content FILENAME | Select-String "SEARCH_TERM" -Context 10,10
```