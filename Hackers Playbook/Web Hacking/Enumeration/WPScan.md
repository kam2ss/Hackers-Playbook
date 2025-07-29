```bash
wpscan --url http://example.com --enumerate u,ap,t
```
- `u` → **Users**: Tries to enumerate WordPress usernames.
- `ap` → **All Plugins**: Scans all plugins (including inactive ones) and checks for known vulnerabilities.
- `t` → **Themes**: Enumerates themes and checks for vulnerabilities.

#### Scan with an API Key
```bash
wpscan --url http://example.com --api-token <API Key>
```