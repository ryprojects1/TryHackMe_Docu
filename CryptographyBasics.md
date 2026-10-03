# Nmap Cheatsheet

## Host Discovery
| Option | Description |
|--------|-------------|
| `-sn` | Ping scan — host discovery only, no port scan |
| `-sL` | List scan — list targets without scanning |
| `-Pn` | Treat all hosts as online (skip host discovery) |

**Local network** — Nmap sends ARP requests, can identify MAC addresses + vendor  
**Remote network** — Can't use ARP (goes through routers), uses TCP/UDP instead

---

## Port Scanning
| Option | Description |
|--------|-------------|
| `-sT` | TCP connect scan — full three-way handshake |
| `-sS` | TCP SYN scan — only first step of handshake (stealthier) |
| `-sU` | UDP scan |
| `-F` | Fast mode — top 100 ports only |
| `-p<range>` | Specific port range e.g. `-p80,443` or `-p-` for all ports |

---

## Service Detection
| Option | Description |
|--------|-------------|
| `-O` | OS detection |
| `-sV` | Service and version detection |
| `-A` | OS + version + extras (aggressive) |

---

## Timing
| Option | Description |
|--------|-------------|
| `-T<0-5>` | 0=paranoid, 1=sneaky, 2=polite, 3=normal, 4=aggressive, 5=insane |
| `--min-rate / --max-rate` | Packets per second |
| `--min-parallelism / --max-parallelism` | Parallel probes |
| `--host-timeout` | Max wait time per host |

---

## Output
| Option | Description |
|--------|-------------|
| `-oN <file>` | Normal output |
| `-oX <file>` | XML output |
| `-oG <file>` | Grep-able output |
| `-oA <basename>` | All formats at once |
| `-v / -vv / -v4` | Verbosity level |
| `-d / -d9` | Debug level (max -d9) |

---

## Common Examples
```bash
nmap -sn 192.168.1.0/24          # discover live hosts on network
nmap -sS -O 192.168.1.10         # SYN scan + OS detection
nmap -sV -p 22,80,443 10.10.1.1  # version detect on specific ports
nmap -A -T4 192.168.1.10         # aggressive scan, fast timing
nmap -sL 192.168.0.0/24          # list 256 targets without scanning
```

# Public Key Cryptography Basics

## Core Principles

### Authentication
You want to be sure you communicate with the right person, not someone else pretending.

### Authenticity
You can verify that the information comes from the claimed source.

### Integrity
You must ensure that no one changes the data you exchange.

### Confidentiality
You want to prevent an unauthorised party from eavesdropping on your conversations.

---

## RSA Encryption

RSA is a public-key encryption algorithm that enables secure data transmission over insecure channels. With an insecure channel, we expect adversaries to eavesdrop on it.

### How RSA Works

**Example:**

Bob chooses two prime numbers:
- `p = 157`
- `q = 199`

He calculates:
- `n = p × q = 31243`
- `ϕ(n) = n − p − q + 1 = 31243 − 157 − 199 + 1 = 30888`

Bob selects:
- `e = 163` (relatively prime to ϕ(n))
- `d = 379` (where e × d = 1 mod ϕ(n))

Verification: `e × d = 163 × 379 = 61777` and `61777 mod 30888 = 1` ✓

**Keys:**
- **Public Key:** `(n, e)` = `(31243, 163)`
- **Private Key:** `(n, d)` = `(31243, 379)`

### Encryption/Decryption

**Alice encrypts message x = 13:**

```
y = x^e mod n = 13^163 mod 31243 = 16341
```

**Bob decrypts:**

```
x = y^d mod n = 16341^379 mod 31243 = 13
```

Bob recovers the original message.

---

## Digital Signature Algorithms

### DSA (Digital Signature Algorithm)
A public-key cryptography algorithm specifically designed for digital signatures.

### ECDSA (Elliptic Curve Digital Signature Algorithm)
A variant of DSA that uses elliptic curve cryptography to provide smaller key sizes for equivalent security.

### ECDSA-SK (ECDSA with Security Key)
An extension of ECDSA that incorporates hardware-based security keys for enhanced private key protection.

### Ed25519
A public-key signature system using EdDSA (Edwards-curve Digital Signature Algorithm) with Curve25519.

### Ed25519-SK (Ed25519 with Security Key)
A variant of Ed25519 that uses hardware-based security keys for improved private key protection.

---

## Certificates: Prove Who You Are!

Certificates are an essential application of public key cryptography, linked to digital signatures. A common place where they're used is **HTTPS**.

### Chain of Trust

Your web browser knows that the server is real through certificates. The flow works like this:

1. **Root CA** (Certificate Authority) is trusted by your browser by default
2. Root CA trusts an **organization**
3. Organization signs a **certificate**
4. Your browser trusts the certificate because it trusts the chain

### Getting TLS Certificates

- **Commercial CAs:** Various certificate authorities charge annual fees
- **Let's Encrypt:** Free TLS certificates for domains you own

### Browser Trust
- [Mozilla Firefox trusted CAs](https://www.mozilla.org/en-US/about/governance/policies/security-group/certs/)
- [Google Chrome trusted CAs](https://support.google.com/chrome/answer/6211280)

---

## GPG (GNU Privacy Guard)

GPG is commonly used in email to:
- Protect the **confidentiality** of email messages
- **Sign** email messages to confirm **authenticity** and **integrity**

---

## Hashing

Hashing plays a vital role in protecting data integrity and ensuring password confidentiality. Hash functions remain hidden from the user but are essential for internet security.

### Hash Collisions

A **hash collision** occurs when two different inputs produce the same output.

Hash functions are designed to:
- Avoid collisions as much as possible
- Prevent attackers from intentionally engineering collisions

**The Pigeonhole Effect:** With unlimited inputs and limited outputs, collisions are mathematically inevitable.

### Rainbow Tables

A **rainbow table** is a lookup table that maps hashes to plaintext values, allowing quick password cracking.

**Trade-off:** Time to crack → Hard disk space (but takes time to create)

**Fast Cracking Sites:**
- [CrackStation](https://crackstation.net/)
- [Hashes.com](https://hashes.com/)

These sites use massive rainbow tables for fast hash lookups without salts.

### Defense: Salting

To protect against rainbow tables, add a **salt** to passwords:

- Salt is a **randomly generated value** stored in the database
- Should be **unique to each user**
- Without per-user salts, duplicate passwords still produce identical hashes
- Attackers could create a rainbow table for passwords with shared salts

### Password Cracking

You can't "decrypt" password hashes — they're not encrypted. You must:

1. Hash many different inputs (e.g., rockyou.txt wordlist)
2. Add salt if one exists
3. Compare to target hash
4. When it matches, you've found the password

**Tools:**
- [Hashcat](https://hashcat.net/hashcat/)
- [John the Ripper](https://www.openwall.com/john/)

---

## Integrity Checking

Hashing verifies that files haven't been modified.

### How It Works

- Same input → Same output always
- Single bit change → Hash changes completely
- Verify file integrity by comparing hashes

### Example

SHA256 hash of a Fedora Workstation ISO file:

```
sha256sum fedora-workstation-live-x86_64-39-1.5.iso
abcd1234... fedora-workstation-live-x86_64-39-1.5.iso
```

If your downloaded file produces the same hash, it's identical to the official version.

---

## HMACs (Keyed-Hash Message Authentication Code)

An **HMAC** uses a cryptographic hash function combined with a secret key to verify authenticity and integrity.

### HMAC Benefits

- **Authenticity:** Secret key proves the creator is who they claim
- **Integrity:** Hashing proves the message hasn't been modified

### How It Works

```
HMAC = hash(secret_key + message)
```

Both parties share the secret key. Only they can create/verify the HMAC.

---

## Encoding vs Encryption

### Encoding
Converts data from one form to another for system compatibility.

**Common Encodings:**
- ASCII
- UTF-8, UTF-16, UTF-32 (Unicode — supports Arabic, Japanese, etc.)
- ISO-8859-1
- Windows-1252

**Note:** Encoding is NOT encryption. It's reversible and provides no security.

---

## Key Takeaways

✓ **Public-key cryptography** enables secure communication over insecure channels  
✓ **RSA, ECDSA, Ed25519** are common algorithms with different trade-offs  
✓ **Certificates** use chain-of-trust to verify identity  
✓ **Hashing** ensures integrity; salting prevents rainbow table attacks  
✓ **HMACs** prove both authenticity and integrity  
✓ **Encoding** ≠ **Encryption** — encoding is reversible and unsecure  

---
#John the ripper: the basics
Where John Comes in
Even though the algorithm is not feasibly reversible, that doesn’t mean cracking the hashes is impossible. If you have the hashed version of a password, for example, and you know the hashing algorithm, you can use that hashing algorithm to hash a large number of words, called a dictionary. You can then compare these hashes to the one you’re trying to crack to see if they match. If they do, you know what word corresponds to that hash- you’ve cracked it!

This process is called a dictionary attack, and John the Ripper, or John as it’s commonly shortened, is a tool for conducting fast brute force attacks on various hash types.

NThash is the hash format modern Windows operating system machines use to store user and service passwords. It’s also commonly referred to as NTLM, which references the previous version of Windows format for hashing passwords known as LM, thus NT/LM.

criteria, many users will use something like the following:

Polopassword1!

Consider the password with a capital letter first and a number followed by a symbol at the end. This familiar pattern of the password, appended and prepended by modifiers (such as capital letters or symbols), is a memorable pattern that people use and reuse when creating passwords. This pattern can let us exploit password complexity predictability.

Now, this does meet the password complexity requirements; however, as attackers, we can exploit the fact that we know the likely position of these added elements to create dynamic passwords from our wordlists.


