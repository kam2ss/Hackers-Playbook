**Count all the Cmdlets installed on the system:**
```
Get-Command -CommandType Cmdlet | Measure-Object
```
or
```
Get-Command | Where-Object -Property CommandType -eq Cmdlet | Measure-Object
```
