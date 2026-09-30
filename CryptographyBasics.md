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
