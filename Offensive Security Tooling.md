
# Hydra — Brute Force Tool

Hydra is an online password cracking tool that brute forces login credentials across multiple protocols.

---

## SSH Brute Force
```bash
hydra -l <username> -P <wordlist_path> <ip> -t 4 ssh
```

| Option | Description |
|--------|-------------|
| `-l` | Single username for login |
| `-P` | Path to password wordlist |
| `-t` | Number of parallel threads |

**Example:**
```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt 10.129.142.238 -t 4 ssh
```

---

## Web Form Brute Force (HTTP POST)
```bash
sudo hydra -l <username> -P <wordlist> <ip> http-post-form "<path>:<login_credentials>:<invalid_response>"
```

| Option | Description |
|--------|-------------|
| `-l` | Username for web form login |
| `-P` | Password wordlist |
| `http-post-form` | Tells Hydra the form uses POST method |
| `<path>` | Login page URL e.g. `/login.php` |
| `<login_credentials>` | Form fields e.g. `username=^USER^&password=^PASS^` |
| `<invalid_response>` | Text that appears on screen when login fails |
| `-V` | Verbose — shows every attempt |

**Example:**
```bash
sudo hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.129.142.238 http-post-form "/login.php:username=^USER^&password=^PASS^:Invalid credentials"
```

---

## How it works
- `^USER^` and `^PASS^` are placeholders — Hydra substitutes the actual username and password from your wordlist on each attempt
- The invalid response string tells Hydra what a **failed** login looks like so it knows when it's found the right password
- Lower `-t` value for SSH to avoid account lockouts or crashing the service

---

> ⚠️ Only use on systems you have permission to test
Enumeration

Enumeration is the act of listing all the available resources, whether they are accessible or not. For example, Gobuster enumerates web directories.

Brute Force

Brute force is the act of trying every possibility until a match is found. It is like having ten keys and trying them all on a lock until one fits. Gobuster uses wordlists for this purpose.
Usage: Shows the syntax on how to use the command.
Available Commands: Multiple commands are available to aid us in enumerating directories, files, DNS subdomains, Google Cloud Storage buckets, and Amazon AWS S3 buckets. Throughout this room, we will focus on the dir, dns, and vhost commands. We will cover each of them in the following tasks.
Flags: These are specific options we can configure to customize our commands. Let’s look at the flags we will often use throughout this room:
Short Flag	Long Flag	Description
-t	--threads	This flag configures the number of threads to use for the scan. Each of these threads sends out requests with a slight delay. The default number of threads is 10. This number may be slow when using large wordlists. You can increase or decrease the number of threads depending on the available system resources.
-w	--wordlist	The flag configures a wordlist to use for iterating. Each wordlist entry is attached to the URL you included in the command.
--delay	This flag defines the amount of time to wait between sending requests. Some web servers include mechanisms to detect enumeration by looking at how many requests are received in a certain period of time. We can increase the delay between subsequent requests to make it look like normal web traffic.
--debug	This flag helps us to troubleshoot when our command gives unexpected errors.
-o	--output	This flag writes the enumeration results to a file we choose.
any flags are used to fine-tune the gobuster dir command. It is out of scope to go over each one of them, but in the table below, we have listed the flags that cover most of the scenarios:

Flag	Long Flag	Description
-c	--cookies	This flag configures a cookie to pass along each request, such as a session ID.
-x	--extensions	This flag specifies which file extensions you want to scan for. E.g., .php, .js
-H	--headers	This flag configures an entire header to pass along with each request.
-k	--no-tls-validation	This flag  skips the process that checks the certificate when https is used. It often happens for CTF events or test rooms like the ones on THM a self-signed certificate is used. This causes an error during the TLS check.
-n	--no-status	You can set this flag when you don’t want to see status codes of each response received. This helps keep the output on the screen clear.
-P	password	You can set this flag together with the --username flag to execute authenticated requests. This is handy when you have obtained credentials from a user.
-s	--status-codes	With this flag, you can configure which status codes of the received responses you want to display, such as 200, or a range like 300-400.
-b	--status-codes-blacklist	This flag allows you to configure which status codes of the received responses you don’t want to display. Configuring this flag overrides the -s flag.
-U	--username	You can set this flag together with the --password flag to execute authenticated requests. This is handy when you have obtained credentials from a user.
-r	--followredirect	This flags configures Gobuster to follow the redirect that it received as a response to the sent request. A HTTP redirect status code (e.g., 301 or 302) is used to redirect the client to a different URL.
The dns mode offers fewer flags than the dir mode. But these are more than enough to cover most DNS subdomain enumeration scenarios. Let us have a look at some of the commonly used flags:

Flag	Long Flag	Description
-c

--show-cname

Show CNAME Records (cannot be used with the -i flag).

-i

--show-ips

Including this flag shows IP addresses that the domain and subdomains resolve to.
-r

--resolver

This flag configures a custom DNS server to use for resolving.
-d

--domain

This flag configures the domain you want to enumerate.
 vhost mode. This mode allows Gobuster to brute force virtual hosts. Virtual hosts are different websites on the same machine.
  vhost mode offers flags similar to those of the dir mode. Let us have a look at some of the commonly used flags:

Short Flag	Long Flag	Description
-u

--url

Specifies the base URL (target domain) for brute-forcing virtual hostnames.

--append-domain

Appends the base domain to each word in the wordlist (e.g., word.example.com).
-m

--method

Specifies the HTTP method to use for the requests (e.g., GET, POST).

--domain

Appends a domain to each wordlist entry to form a valid hostname (useful if not provided explicitly).

--exclude-length

Excludes results based on the length of the response body (useful to filter out unwanted responses).
-r

--follow-redirect

Follows HTTP redirects (useful for cases where subdomains may redirect).
