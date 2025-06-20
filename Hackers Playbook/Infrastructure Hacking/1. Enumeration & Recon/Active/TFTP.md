Files cannot be listed over TFTP, but with knowledge of a file, once connected can perform `get file.txt`.
```
# Connect to the TFTP server
tftp 192.168.1.1

# get the token file
get token.txt
```

The above command should download the `token.txt` file in your current directory in your local machine.