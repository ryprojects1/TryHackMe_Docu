
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
