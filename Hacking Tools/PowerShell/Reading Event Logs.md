### Analysing log files with PowerShell
Event logs are an important element of fault diagnosis or incident response. Desktop-based Windows Operating Systems have a GUI event log viewer that can be used to query log files. On Server Core images or remote systems, using a GUI may not be possible.

PowerShell comes with the `Get-EventLog` cmdlet that allows you to query event logs from the command line. By default, it will query the local machine; however, it can also query logs from remote connections. It has several options that can be used to filter the query, and, similar to most PowerShell, the output can be piped to other filters like search and output. 
```powershell
Get-Help Get-EventLog
```

### Viewing Logs
To view logs using the `Get-EventLog` cmdlet, you first need to select a log. To see the list of logs and their entries, you can use the `-List` option:
```powershell
# Lists the available logs 
Get-EventLog -list
 
# Shows all entries for a specific log
Get-EventLog System
```

### Filter Logs
The default behaviour of `Get-EventLog` is to ***list all log entries in order of date***. We can filter the log entries based on a number of fields to help with finding the relevant log entries we are looking for:
- Date
- EntryType
- Source
- InstanceID
- Message

The following example filters logs received in the last hour:
```powershell
Get-EventLog System -After (get-date).addhours(-1)
```

We're also able to add additional filters and groupings; for example, to group the last 1000 events by their source:
```powershell
Get-EventLog -LogName System -Newest 1000 | Group-Object -Property Source -noelement | Sort-Object -Property count -descending
```

### Export Event Logs
Sometimes it is easier to analyze event logs in other tools or to attach them to event tickets. For this, we can export the logs to other types. The following example exports the application logs to CSV:
```powershell
Get-EventLog Application | Export-Csv -Path application.csv
```
