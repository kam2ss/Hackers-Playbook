Burp Intruder is a tool within Burp Suite package used to ***automate attacks on web applications***. It supports a variety of attacks, from ***brute force*** to exploiting complex vulnerabilities like ***blind SQL Injection***.

The process starts with capturing an HTTP request using Burp proxy and sending it to Intruder. Users can then ***define payloads and their positions in the request***, using various generation methods and placement algorithms.

**Uses**
Using Intruder, attackers can ***enumerate the identifiers*** on a website, such as ***items, products, or users***, to automate the process and filter results based on specific strings.

### Configurations
- **Target:** Allows you to ***specify your target*** by providing hostname, port, and whether HTTP or HTTPs is used.
- **Positions:** Allows configuration of ***payload order and attack types***. Payloads are marked with `§` sign, and Intruder automatically highlights potential payloads.
- **Payloads:** Configures the ***payload types***. For brute forcing or fuzzing, often a single payload is used, but larger wordlists may be needed for specific attacks, like password spraying.
- **Options:** Customizes the attack further, with the ***ability to flag*** or ***extract specific strings*** from the response.

### Attack Types
Intruder offer different attack types:
- **Sniper:** uses `one payload` set, testing each value individually in a single position. Best of `fuzzing`.
- **Battering Ram:** `one payload` set is reused in all marked positions simultaneously. `Ideal when the same input is required in multiple request fields.`
- **Pitchfork:** `multiple payload` set is used in ***parallel*** (one per position) advancing together step-by-step. `Useful for password spraying`.
- **Cluster Bomb:** ***multiple payload*** sets with all possible combinations across positions. `best for full brute force when credentials are unknown`.

**Important Note**
When performing ***Brute Force*** with a username or password file, look for a difference in either `Status` or `Length` in Intruder tab.