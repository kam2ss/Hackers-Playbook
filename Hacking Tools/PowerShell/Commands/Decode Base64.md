**Using the Base64 Output String**
```
[System.Taxt.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("base64string"))
```

**Using a Base64-Encoded File**
```
$encoded = Get-Content BASE64.txt
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encoded))
```
