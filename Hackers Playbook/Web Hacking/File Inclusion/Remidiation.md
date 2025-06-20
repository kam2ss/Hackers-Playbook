To prevent the file inclusion vulnerabilities, some common suggestions are:
- keep the system updated, including web application frameworks
- Turn off PHP errors to avoid leaking the path
- WAF is a good option
- Disable some PHP features if the web app doesn't need them, such as `allow_url_fopen` on and `allow_url_include`
- Allow only protocols and PHP wrappers that are needed
- Implement proper user input validation
- Implement whitelisting for file names and locations as well as blacklisting