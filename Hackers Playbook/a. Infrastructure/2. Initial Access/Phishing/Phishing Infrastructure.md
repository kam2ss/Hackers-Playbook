A certain amount of infrastructure will need to be put in place to launch a successful phishing campaign.

**Domain Name:**
You need to register either an authentic-looking domain name or one that mimics the identity of another domain.

**SSL/TLS Certificates:**
Creating SSL/TLS certificates for your chosen domain name will add an extra layer of authenticity to the attack.

**Email Server/Account:**
You need to either setup an email server or register with an SMTP email provider.

**DNS Records:**
Setting up DNS Records such as `SPF, DKIM, DMARC` will improve the deliverability of your emails and make sure they're getting into the inbox rather than the spam folder.

**Web Server:**
You need to setup webservers or purchase web hosting from a company to host your phishing websites. Adding SSL/TLS to the websites will give them an extra layer of authenticity.

**Analytics:**
You need something to keep track of emails that have been sent, opened, or clicked. You need to combine it with information from your phishing websites for which users have supplied personal information or downloaded software.

**Automation and Useful Software**
- `GoPhish:` [getgophish.com](https://getgophish.com/) is a web-based framework that allows you to store your SMTP server settings for sending emails. You can also schedule when emails are sent and have an analytics dashboard that shows how many emails have been sent, opened, or clicked.

- `SET (Social Engineering Toolkit):` [trustedsec.com](https://www.trustedsec.com/tools/the-social-engineer-toolkit-set/) Social Engineering Toolkit contains a multitude of tools, but some of the important ones for phishing are the ability to ***create spear-phishing attacks and deploy fake versions of common websites*** to trick victims into entering their credentials.