### What is WPScan?
WPScan is an automated black box WordPress vulnerability scanner that comes pre-packaged in Kali Linux. It is formed of two parts: the `WPScan Vulnerability Database (WpVulnDB)` and `WPScan` itself. The WPScan Vulnerability Database is a website that does not contain any exploit code; however, with some versions of WordPress and WordPress plugins, WPScan will provide links to known vulnerabilities. 

#### Basic scan
In order to run a basic vulnerability scan you can run the following:
```bash
wpscan [OPTIONS] --url [IP Address/URL]
```

#### User enumeration
Using the `-e u` or `--enumerate u` option, WPScan can generate a list of usernames. With this list of usernames, there is also a brute force plugin that will try wordlists against each of the discovered accounts.

```bash
wpscan -e u --url [IP Address/URL]
```

#### Plugin scan
Using the `-e p` or `--enumerate p` option, WPScan will look for installed plugins and attempt to get their version number. The default option is to only scan for common plugins that have a vulnerability. You can override this with the `-e ap` option. If you are using Version 3.x, you can also change the method used for detection using the `--plugins-detection` aggressive option.

**Note: If WPScan does not return any plugins, use the --plugins-detection aggressive switch.**
```bash
wpscan -e p --url [IP Address/URL]wpscan --url [IP address/RUL] --enumerate ap --plugins-detection aggressive
```

#### Themes enumeration
Using the `-e t` or `--enumerate t` option, WPScan will look for installed themes and list any vulnerabilities that it finds.
```bash
wpscan -e t --url [IP Address/URL]
```

### WPScan Version 3
WPScan has been revamped since this lab was created, moving from versions 2.x to 3.x. We have updated the WPSscan version in our Kali client to match the latest version. Ensure you read the help articles for any differences in the syntax or any extra options you need to set.

## Brute force
You can use WPScan to iterate over a password list and try to guess a user’s password via the `--passwords` option, passing it a wordlist of your choice. There are many preset wordlists for Kali users in the `/usr/share/wordlists/` directory.
```bash
wpscan --url [IP Address/URL] --passwords passwords.txt
```
