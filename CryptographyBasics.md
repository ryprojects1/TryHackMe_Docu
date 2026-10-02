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

Public Key Cryptography Basics
Authentication: You want to be sure you communicate with the right person, not someone else pretending.
Authenticity: You can verify that the information comes from the claimed source.
Integrity: You must ensure that no one changes the data you exchange.
Confidentiality: You want to prevent an unauthorised party from eavesdropping on your conversations.

RSA is a public-key encryption algorithm that enables secure data transmission over insecure channels. With an insecure channel, we expect adversaries to eavesdrop on it.

Bob chooses two prime numbers: p = 157 and q = 199. He calculates n = p × q = 31243.
With ϕ(n) = n − p − q + 1 = 31243 − 157 − 199 + 1 = 30888, Bob selects e = 163 such that e is relatively prime to ϕ(n); moreover, he selects d = 379, where e × d = 1 mod ϕ(n), i.e., e × d = 163 × 379 = 61777 and 61777 mod 30888 = 1. The public key is (n,e), i.e., (31243,163) and the private key is $(n,d), i.e., (31243,379).
Let’s say that the value they want to encrypt is x = 13, then Alice would calculate and send y = xe mod n = 13163 mod 31243 = 16341.
Bob will decrypt the received value by calculating x = yd mod n = 16341379 mod 31243 = 13. This way, Bob recovers the value that Alice sent.

DSA (Digital Signature Algorithm) is a public-key cryptography algorithm specifically designed for digital signatures.
ECDSA (Elliptic Curve Digital Signature Algorithm) is a variant of DSA that uses elliptic curve cryptography to provide smaller key sizes for equivalent security.
ECDSA-SK (ECDSA with Security Key) is an extension of ECDSA. It incorporates hardware-based security keys for enhanced private key protection.
Ed25519 is a public-key signature system using EdDSA (Edwards-curve Digital Signature Algorithm) with Curve25519.
Ed25519-SK (Ed25519 with Security Key) is a variant of Ed25519. Similar to ECDSA-SK, it uses a hardware-based security key for improved private key protection.

Certificates: Prove Who You Are!
Certificates are an essential application of public key cryptography, and they are also linked to digital signatures. A common place where they’re used is for HTTPS. How does your web browser know that the server you’re talking to is the real tryhackme.com?

The answer lies in certificates. The web server has a certificate that says it is the real tryhackme.com. The certificates have a chain of trust, starting with a root CA (Certificate Authority). From install time, your device, operating system, and web browser automatically trust various root CAs. Certificates are trusted only when the Root CAs say they trust the organisation that signed them. In a way, it is a chain; for example, the certificate is signed by an organisation, the organisation is trusted by a CA, and the CA is trusted by your browser. Therefore, your browser trusts the certificate. In general, there are long chains of trust. You can take a look at the certificate authorities trusted by Mozilla Firefox here(opens in new tab) and by Google Chrome here(opens in new tab).

Let’s say you have a website and want to use HTTPS. This step requires having a TLS certificate. You can get one from the various certificate authorities for an annual fee. Furthermore, you can get your own TLS certificates for domains you own using Let's Encrypt(opens in new tab) for free. If you run a website, it’s worth setting up and switching to HTTPS, as any modern website would do.

GPG is commonly used in email to protect the confidentiality of the email messages. Furthermore, it can be used to sign an email message and confirm its integrity.

Hashing plays a vital role in our daily use of the Internet. Like other cryptographic functions, hashing remains hidden from the user. Hashing helps protect data’s integrity and ensure password confidentiality.

A hash collision is when two different inputs give the same output. Hash functions are designed to avoid collisions as best as possible. Furthermore, hash functions are designed to prevent an attacker from being able to create, i.e., engineer, a collision intentionally. However, because the number of inputs is practically unlimited and the number of possible outputs is limited, this leads to a pigeonhole effect.

The pigeonhole effect states that the number of items (pigeons) is more than the number of containers (pigeonholes)

A Rainbow Table is a lookup table of hashes to plaintexts, so you can quickly find out what password a user had just from the hash. A rainbow table trades the time to crack a hash for hard disk space, but it takes time to create. Here’s a quick example to get an idea of what a rainbow table looks like.

Websites like CrackStation(opens in new tab) and Hashes.com(opens in new tab) internally use massive rainbow tables to provide fast password cracking for hashes without salts. Doing a lookup in a sorted list of hashes is quicker than trying to crack the hash

To protect against rainbow tables, we add a salt to the passwords. The salt is a randomly generated value stored in the database and should be unique to each user. In theory, you could use the same salt for all users, but duplicate passwords would still have the same hash and a rainbow table could still be created for passwords with that salt.

You can’t “decrypt” password hashes. They’re not encrypted. You have to crack the hashes by hashing many different inputs (such as rockyou.txt as it covers many possible passwords), potentially adding the salt if there is one and comparing it to the target hash. Once it matches, you know what the password was. Tools like Hashcat(opens in new tab) and John the Ripper(opens in new tab) are commonly used for these purposes.
Integrity Checking
Hashing can be used to check that files haven’t been changed. If you put the same data in, you always get the same data out. Even if a single bit changes, the hash will change significantly, as demonstrated in Task 2. This means you can use it to check that files haven’t been modified or to ensure that the file you downloaded is identical to the file on the web server. The text file listed below shows the SHA256 hash of two Fedora Workstation ISO files. If running sha256sum on the file you downloaded returned the same hash listed in this signed file, you can be confident that your file is identical to the official one.HMACs
HMAC (Keyed-Hash Message Authentication Code) is a type of message authentication code (MAC) that uses a cryptographic hash function in combination with a secret key to verify the authenticity and integrity of data.

An HMAC can be used to ensure that the person who created the HMAC is who they say they are, i.e., authenticity is confirmed; moreover, it proves that the message hasn’t been modified or corrupted, i.e., integrity is maintained. This is achieved through the use of a secret key to prove authenticity and a hashing algorithm to produce a hash and prove integrity.

Encoding converts data from one form to another to make it compatible with a specific system. ASCII, UTF-8, UTF-16, UTF-32, ISO-8859-1, and Windows-1252 are valid encoding methods for the English language. Note that UTF-8, UTF-16, and UTF-32 are Unicode encodings, and they can represent characters from other languages, such as Arabic and Japanese.

