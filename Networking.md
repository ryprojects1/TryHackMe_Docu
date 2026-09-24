# Networking Fundamentals

Quick reference for OSI Model, TCP/IP, and network basics.

---

## OSI Model (7 Layers)

| Layer | Name | Function | Examples |
|-------|------|----------|----------|
| 7 | Application | Services & interfaces | HTTP, FTP, DNS, SMTP, POP3 |
| 6 | Presentation | Encoding, encryption, compression | Unicode, MIME, JPEG, PNG |
| 5 | Session | Establish & maintain sessions | NFS, RPC |
| 4 | Transport | End-to-end communication | TCP, UDP |
| 3 | Network | Logical addressing & routing | IP, ICMP, IPSec |
| 2 | Data Link | Reliable adjacent node transfer | Ethernet, WiFi (802.11) |
| 1 | Physical | Physical transmission media | Electrical, optical, wireless |

---

## TCP/IP Model

Simplified version of OSI - combines some layers:

- **Application Layer** = OSI layers 5, 6, 7
- **Transport Layer** = OSI layer 4 (TCP, UDP)
- **Internet Layer** = OSI layer 3 (IP, ICMP)
- **Link Layer** = OSI layer 2 (Ethernet, WiFi)

---

## Key Concepts

### Private IP Ranges (RFC 1918)
```
10.0.0.0 - 10.255.255.255 (10/8)
172.16.0.0 - 172.31.255.255 (172.16/12)
192.168.0.0 - 192.168.255.255 (192.168/16)
```

### Port Numbers
- Valid range: 1 - 65535 (uses 2 octets)
- Port 0 is reserved
- Identifies processes/services on a device

### TCP Three-Way Handshake
How TCP establishes connection:
1. SYN (client sends sync request)
2. SYN-ACK (server acknowledges)
3. ACK (client acknowledges back)

### Encapsulation
Each layer adds a header (and sometimes trailer) to data:
- Application data → Transport layer adds TCP/UDP header → Segment/Datagram
- Gets passed down the stack with each layer adding its own header

---

## Protocols & Services

**TELNET**
- Teletype Network protocol
- Allows remote terminal connection
- Text-based commands over TCP
- Originally for remote admin (now less secure, use SSH)

**Common Lab Services:**
- Echo server (port 7) - echoes everything you send
- Daytime server (port 13) - sends current time/date
- HTTP web server (port 80) - serves web pages

---

#Networking Essentials
# Network Protocols: DHCP, ARP, ICMP & Routing

Essential protocols for network communication, device discovery, and diagnostics.

---

## DHCP (Dynamic Host Configuration Protocol)

**What it is:** Application-level protocol that automatically assigns IP addresses to devices.

**How it works:**
- Server listens on UDP port 67
- Client sends from UDP port 68
- Used by default on smartphones and laptops

### DORA Process (4 Steps)

| Step | Packet | Direction | Purpose |
|------|--------|-----------|---------|
| 1. Discover | DHCPDISCOVER | Client → Broadcast | Client seeks DHCP server |
| 2. Offer | DHCPOFFER | Server → Client | Server offers available IP |
| 3. Request | DHCPREQUEST | Client → Broadcast | Client accepts the IP |
| 4. Acknowledge | DHCPACK | Server → Client | Server confirms IP assignment |

### Key Detail
Client starts with only a MAC address (no IP yet). In steps 1 & 3:
- Sends from IP `0.0.0.0` to broadcast `255.255.255.255`
- Uses broadcast MAC `ff:ff:ff:ff:ff:ff`
- Server responds using client's MAC address

---

## ARP (Address Resolution Protocol)

**What it is:** Finds MAC addresses of devices on the local network (Ethernet).

**How it works:**
```
Host A (192.168.66.89) → "Who has 192.168.66.1?" (broadcast)
Host B (192.168.66.1) → "That's me at MAC 44:df:65:d8:fe:6c" (unicast)
```

**Example:**
```
1 cc:5e:f8:02:21:a7 → ff:ff:ff:ff:ff:ff  Who has 192.168.66.1? Tell 192.168.66.89
2 44:df:65:d8:fe:6c → cc:5e:f8:02:21:a7  192.168.66.1 is at 44:df:65:d8:fe:6c
```

---

## ICMP (Internet Control Message Protocol)

**What it is:** Used for network diagnostics and error reporting.

### Ping
- Tests connectivity to a target
- Measures round-trip time (RTT)
- Determines if target is alive

### Traceroute
**Linux/Unix:** `traceroute`  
**Windows:** `tracert`

**How it works:**
- Uses Time-to-Live (TTL) field in IP packets
- Each router decrements TTL by 1
- When TTL reaches 0, router sends ICMP Type 11 (Time Exceeded)
- Reveals routers between your host and target

**What you learn:**
- ISP routers (may show private IPs)
- Public routers (can look up domain & location)
- Non-responding routers (drop packets silently)
- Some ICMP messages get blocked

---

## Routing Protocols

How routers communicate and choose paths for data.

### OSPF (Open Shortest Path First)
- Routers share network topology information
- Calculates most efficient paths
- Each router builds complete network map
- Good for large, complex networks

### EIGRP (Enhanced Interior Gateway Routing Protocol)
- Cisco proprietary protocol
- Combines multiple routing algorithms
- Routers share network reachability + cost info (bandwidth, delay)
- Faster convergence than OSPF

### BGP (Border Gateway Protocol)
- Primary routing protocol for the Internet
- Used between ISPs and large networks
- Exchanges routing info between different networks
- Ensures efficient routing across the entire Internet

### RIP (Routing Information Protocol)
- Simple protocol for small networks
- Routers share reachable networks + hop count
- Chooses routes with fewest hops
- Older, less efficient than modern protocols

---

## Quick Comparison

| Protocol | Purpose | Layer | Port(s) | Use Case |
|----------|---------|-------|---------|----------|
| DHCP | Auto IP assignment | Application (UDP) | 67/68 | Dynamic host config |
| ARP | Find MAC addresses | Link | — | Local network discovery |
| ICMP | Diagnostics | Network | — | Ping, traceroute |
| OSPF | Routing | Network | — | Large networks |
| BGP | Internet routing | Network | — | ISP ↔ ISP |


#Networking Core Protocols

A record: The A (Address) record maps a hostname to one or more IPv4 addresses. For example, you can set example.com to resolve to 172.17.2.172.
AAAA Record: The AAAA record is similar to the A Record, but it is for IPv6. Remember that it is AAAA (quad-A), as AA and AAA would refer to a battery size; furthermore, AAA refers to Authentication, Authorization, and Accounting; neither falls under DNS.
CNAME Record: The CNAME (Canonical Name) record maps a domain name to another domain name. For example, www.example.com can be mapped to example.com or even to example.org.
MX Record: The MX (Mail Exchange) record specifies the mail server responsible for handling emails for a domain.

, we have used the whois command to look up a domain whose WHOIS record is protected by privacy protection.

user@TryHackMe$ whois [REDACTED].com
[...]
Domain Name: [REDACTED].COM
Registry Domain ID: [REDACTED]
Registrar WHOIS Server: whois.godaddy.com
Registrar URL: https://www.godaddy.com
Updated Date: 2017-07-05T16:02:43Z
Creation Date: 1993-04-02T00:00:00Z
Registrar Registration Expiration Date: 2026-10-20T14:56:17Z
Registrar: GoDaddy.com, LLC
Registrar IANA ID: 146
Registrar Abuse Contact Email: abuse@godaddy.com
Registrar Abuse Contact Phone: +1.4806242505
[...]
Registrant Name: Registration Private
Registrant Organization: Domains By Proxy, LLC
Registrant Street: DomainsByProxy.com
[...]

Unlike HTTP, which is designed to retrieve web pages, File Transfer Protocol (FTP) is designed to transfer files. As a result, FTP is very efficient for file transfer, and when all conditions are equal, it can achieve higher speeds than HTTP.

Example commands defined by the FTP protocol are:

USER is used to input the username
PASS is used to enter the password
RETR (retrieve) is used to download a file from the FTP server to the client.
STOR (store) is used to upload a file from the client to the FTP server.
FTP server listens on TCP port 21 by default; data transfer is conducted via another connection from the client to the server.

In the terminal below we executed the command ftp 10.129.166.175 to connect to the remote FTP server using the local ftp client. Then we went through the following steps:

# Networking Core Protocols

## DNS Records
- **A Record** — Hostname → IPv4 address
- **AAAA Record** — Hostname → IPv6 address
- **CNAME Record** — Domain → another domain
- **MX Record** — Specifies mail server for a domain

## WHOIS
Lookup domain registration info. Privacy protection replaces owner details with a proxy service.
```bash
whois example.com
```

## FTP — Port 21
Designed for file transfer. Uses two connections: port 21 (control) + dynamic port (data).

| Command | Description |
|---------|-------------|
| `USER` / `PASS` | Login credentials |
| `RETR` | Download file |
| `STOR` | Upload file |
| `get <file>` | Retrieve file |

Anonymous login: username `anonymous`, no password required.  
⚠️ Plaintext protocol — use SFTP/FTPS for secure transfer.

## SMTP — Sending Email
| Command | Description |
|---------|-------------|
| `EHLO` | Start session |
| `MAIL FROM` | Sender address |
| `RCPT TO` | Recipient address |
| `DATA` | Begin message content |
| `.` | End of message |

## POP3 — Receiving Email (downloads & deletes from server)
| Command | Description |
|---------|-------------|
| `USER` / `PASS` | Authenticate |
| `LIST` | List messages |
| `RETR <n>` | Get message |
| `DELE <n>` | Delete message |
| `QUIT` | End session |

## IMAP — Receiving Email (syncs across devices)
| Command | Description |
|---------|-------------|
| `LOGIN` | Authenticate |
| `SELECT <mailbox>` | Open folder |
| `FETCH <n> body[]` | Get message |
| `MOVE` / `COPY` | Move or copy messages |
| `LOGOUT` | End session |

## POP3 vs IMAP
| | POP3 | IMAP |
|--|------|------|
| Storage | Local | Server |
| Multi-device | ❌ | ✅ |
| Sync | ❌ | ✅ |

## Secure Alternatives
| Protocol | Secure Version | Port |
|----------|---------------|------|
| FTP | SFTP/FTPS | 22/990 |
| SMTP | SMTP+TLS | 587 |
| POP3 | POP3S | 995 |
| IMAP | IMAPS | 993 |

# Secure Network Protocols: TLS, SSH & VPN

Three main approaches to securing network traffic over insecure networks.

---

## TLS/SSL (Transport Layer Security)

**What it is:** Cryptographic protocol at OSI transport layer that encrypts communication between client and server.

**Why it matters:** Ensures confidentiality (can't read) and integrity (can't modify) of network packets.

**How it works:**
1. Server gets signed TLS certificate from Certificate Authority (CA)
2. Certificate proves server identity
3. Browser verifies certificate using trusted CAs
4. Secure connection established

### Secured Protocols (Adding "S")

Protocols that added TLS security:
- HTTP → **HTTPS**
- DNS → **DoT** (DNS over TLS)
- SMTP → **SMTPS**
- POP3 → **POP3S**
- MQTT → **MQTTS**

The "S" stands for Secure.

### HTTPS Connection Steps

After DNS resolution:
1. TCP three-way handshake
2. Establish TLS session
3. Communicate via HTTP (GET, POST, etc.)

---

## SSH (Secure Shell)

**History:**
- Created by Tatu Ylönen (1995) to replace Telnet
- SSH-2 defined in 1996
- OpenSSH released by OpenBSD (1999) - now industry standard

**Why replace Telnet?** Telnet sends everything in plaintext (including passwords). SSH encrypts everything.

### Key Benefits

✅ **Secure Authentication**
- Password-based
- Public key authentication
- Two-factor authentication

✅ **Confidentiality**
- End-to-end encryption
- Protection against eavesdropping
- Alerts on new/suspicious server keys

✅ **Integrity**
- Cryptography protects data integrity
- Detects tampering

✅ **Tunneling**
- Create secure tunnels for other protocols
- VPN-like connections
- Useful for plaintext protocols (like VNC)

✅ **X11 Forwarding**
- Use graphical apps remotely over SSH

---

## VPN (Virtual Private Network)

**What it is:** Encrypted tunnel connecting remote locations over the Internet while keeping data "private."

**Why it matters:** TCP/IP designed for routing, not security. VPN adds the security layer.

**How it works:**

```
Remote Branch         Main Branch
   VPN Client  =======(encrypted tunnel)=======> VPN Server
```

- Client encrypts traffic before sending
- All traffic goes through encrypted tunnel
- VPN server decrypts and forwards to real destination
- ISP only sees encrypted data (can't censor or inspect)
- Original public IP hidden from services (they see VPN server's IP)

### Common Uses

✅ **Company networks** - Connect remote branches securely
✅ **Geographic spoofing** - Appear to be in different location (like connecting via Japan VPN)
✅ **Bypassing restrictions** - ISP can't see/block encrypted traffic
✅ **Privacy** - Services see VPN server IP, not your real IP

---

## Comparison: Which One to Use?

| Approach | Best For | Pros | Cons |
|----------|----------|------|------|
| **TLS** | Single protocol security | Simple, widely supported | Per-protocol setup |
| **SSH** | Remote access & tunneling | Flexible, multi-auth options | Mainly for terminal |
| **VPN** | Entire network privacy | All traffic encrypted, easy setup | Requires VPN server |

---

## Real-World Scenario

**Insecure (Old Way):**
- Telnet login → password sent plaintext → anyone sniffing network sees it ❌

**Secure (Modern Way):**
- SSH login → encrypted connection → password protected ✅
- HTTPS browsing → encrypted connection → data private ✅
- VPN tunnel → all traffic encrypted → ISP can't see what you're doing ✅

---

## TLS Certificates Deep Dive

**How it works:**
1. Server admin creates Certificate Signing Request (CSR)
2. Submits to Certificate Authority (CA) for verification
3. CA issues digitally signed certificate
4. Server uses certificate to prove identity
5. Client verifies certificate using trusted CAs (like browser's CA store)

Think of it like passport stamps - the CA's signature proves authenticity.
# Wireshark: The Basics

Wireshark is an open-source, cross-platform network packet analyser. It sniffs live traffic and inspects packet captures (PCAP files), making it one of the best tools for understanding what's actually moving across your network.

## Why Wireshark Matters

**Network Troubleshooting**
- Find where traffic is breaking or slowing down
- Spot congestion and load issues

**Security Analysis**
- Detect rogue hosts and suspicious activity
- Catch abnormal port usage
- Identify intrusion patterns

**Protocol Learning**
- See exactly how protocols work
- Inspect response codes, payloads, headers
- Understand the packets you're blocking with your firewall

## The Interface

### Toolbar
Main menu and shortcuts. Use this for packet capture, filtering, sorting, exporting.

### Display Filter Bar
Where you write queries to filter packets. This is your power tool.

### Capture Filter & Interfaces
Choose which network interface to sniff on (eth0, ens33, lo, etc). Capture filters decide what gets recorded.

### Packet List Pane
Summary view — shows source/destination, protocol, and basic packet info. Click a packet to dive deeper.

### Packet Details Panel
Protocol breakdown. Shows exactly what's in each layer (TCP, IP, HTTP, etc).

### Packet Bytes Pane
Raw hex and ASCII. The ground truth of what's in the packet.

### Status Bar
Shows total packets, displayed packets, profile info.

## Packet Dissection

**What it is:** Breaking down a packet to see all its layers and fields.

Wireshark supports hundreds of protocols. It automatically decodes them so you don't have to read raw hex. You can also write custom dissection scripts.

## Finding Packets

### By Packet Number
Use "Go → Go to Packet" to jump to a specific packet number. Useful for frame tracking and following conversations.

### By Content
"Edit → Find Packet" searches inside packets for specific patterns.

**Search types:**
- **String** — Search for text (case-insensitive by default)
- **Regex** — Pattern matching (for complex searches)
- **Hex** — Raw bytes
- **Display Filter** — Use Wireshark's query language

**Pro tip:** Know which pane has the info you're looking for. Searching packet details in the packet list pane won't work.

## Filtering: The Golden Rule

**"If you can click on it, you can filter on it."**

Wireshark has two filter types:

### Capture Filters
Applied during capture. Only saves packets matching the filter. Efficient but you lose data outside the filter.

### Display Filters
Applied after capture. Shows/hides packets without deleting them. This is what you'll use most.

## Filtering Workflows

### Right-Click Method (Easiest)
1. Click a field in the packet details
2. Right-click → "Apply as Filter"
3. Boom. Filtered.

### Menu Method
Click a field → "Analyse → Apply as Filter"

### Manual Queries
Type directly in the Display Filter bar.

**Examples:**
```
ip.src == 192.168.1.100          # Traffic from specific IP
tcp.port == 22                   # SSH traffic
http.request.method == "POST"    # HTTP POST requests
dns.qry.name contains "google"   # DNS queries for google
```

## Quick Wins

**Isolate one conversation:**
- Right-click a packet → Follow → TCP Stream (or UDP/HTTP)
- Shows you only packets in that connection

**Export packets:**
- File → Export → Choose format (CSV, JSON, etc)
- Good for reports or feeding into other tools

**Colorize traffic:**
- View → Coloring Rules
- Red for suspicious, green for known-good
- Makes patterns jump out

# Tcpdump: The Basics

Tcpdump is a command-line packet capture and analysis tool. It's lightweight, blazingly fast, and the foundation for Wireshark and most other network analysis tools. Written in C/C++ and released in the late 1980s for Unix systems, it's battle-tested and stable.

Why learn tcpdump when you have Wireshark? Because tcpdump runs on servers without GUIs, on remote systems over SSH, and in scripts. Wireshark is the GUI; tcpdump is the engine.

## Why You Need Tcpdump

**On your homelab:**
- Monitor traffic in real-time from the command line
- Capture packets to a file for later analysis in Wireshark
- Write complex filters to isolate exactly the traffic you care about
- Automate packet capture in scripts

**In penetration testing / incident response:**
- Capture live traffic on remote systems
- Hunt for intrusion signatures
- Analyze network attacks as they happen
- Build packet captures for forensics

## Choosing a Network Interface

Every packet capture starts with picking which network interface to listen on.

```bash
# List available interfaces
ip address show
# or
ifconfig
```

Common interface names:
- `eth0`, `ens33` — Ethernet
- `wlan0` — WiFi
- `lo` — Loopback (localhost traffic)
- `any` — All interfaces (handy but slower)

**Capture on a specific interface:**
```bash
sudo tcpdump -i eth0
```

**Capture on all interfaces:**
```bash
sudo tcpdump -i any
```

**Why sudo?** Packet capture requires root privileges. You're reading raw network data.

## Saving and Reading Packets

### Capture to a File

```bash
sudo tcpdump -i eth0 -w capture.pcap
```

This saves packets to `capture.pcap` without printing anything to screen. You won't see the traffic scrolling — it's all going to the file. Perfect for long captures or when you want to analyze later in Wireshark.

### Read from a File

```bash
tcpdump -r capture.pcap
```

This reads a saved file and prints packets to stdout. Useful for:
- Inspecting old captures
- Applying filters to a file you already have
- Learning how protocols work offline

## Limiting Packet Count

By default, tcpdump captures forever until you press Ctrl+C. You can limit it:

```bash
# Capture exactly 100 packets, then stop
sudo tcpdump -i eth0 -c 100
```

Great for quick tests where you know roughly how much data you need.

## Output Control

Tcpdump can be verbose or quiet. Control it with flags:

```bash
tcpdump -q           # Quiet: source, dest, protocol only
tcpdump -v           # Verbose: more detail
tcpdump -vv          # Very verbose
tcpdump -vvv         # Extremely verbose
tcpdump -n           # Don't resolve IP addresses to hostnames
tcpdump -nn          # Don't resolve IPs or port numbers
```

## Filtering by Host

### All traffic to/from a host

```bash
sudo tcpdump host 192.168.1.100
sudo tcpdump host example.com
```

### Only outgoing traffic from a host

```bash
sudo tcpdump src host 192.168.1.100
```

### Only incoming traffic to a host

```bash
sudo tcpdump dst host 192.168.1.100
```

## Filtering by Port

### All traffic on a specific port

```bash
sudo tcpdump port 22          # SSH
sudo tcpdump port 80          # HTTP
sudo tcpdump port 53          # DNS
```

### Only traffic FROM a specific source port

```bash
sudo tcpdump src port 8080
```

### Only traffic TO a specific destination port

```bash
sudo tcpdump dst port 443     # HTTPS
```

## Filtering by Protocol

Capture only certain types of traffic:

```bash
sudo tcpdump tcp              # TCP packets only
sudo tcpdump udp              # UDP packets only
sudo tcpdump icmp             # ICMP (ping, traceroute)
sudo tcpdump ip               # All IPv4
sudo tcpdump ip6              # All IPv6
sudo tcpdump arp              # ARP traffic
```

**Real example:** Capture all DNS queries

```bash
sudo tcpdump port 53 -n
# -n prevents reverse DNS lookups that would slow this down
```

## Packet Size Filtering

Useful for finding jumbo packets or tiny packets:

```bash
sudo tcpdump greater 1000     # Packets >= 1000 bytes
sudo tcpdump less 100         # Packets <= 100 bytes
```

## Display Packet Contents

### ASCII View

```bash
sudo tcpdump -A
```

Shows packet data as ASCII text. Works great if the payload is plain text (HTTP, etc). Garbage output if encrypted or binary.

### Hexadecimal View

```bash
sudo tcpdump -xx
```

Shows every byte as hex. Two hex digits = one octet. Useful for:
- Encrypted traffic (you can't read it anyway, but you see the pattern)
- Binary protocols
- Reverse engineering

### Hex + ASCII (Best of Both)

```bash
sudo tcpdump -X
```

Shows hex on the left, ASCII on the right. Easiest to read.

### MAC Addresses

```bash
sudo tcpdump -e
```

Includes Ethernet MAC addresses. Useful for learning ARP, DHCP, and tracking devices on the network.

## TCP Flags

TCP flags tell you what kind of packet it is. The main ones:

- **SYN** — Connection start (three-way handshake begins)
- **ACK** — Acknowledgement (data received)
- **FIN** — Connection end
- **RST** — Reset (connection abort)
- **PUSH** — Send data now

You can filter by flags using binary operations:

```bash
# Capture packets with SYN flag set
sudo tcpdump "tcp[tcpflags] & tcp-syn != 0"

# Capture packets with BOTH SYN and ACK
sudo tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"

# Capture only pure SYN packets (SYN but not ACK)
sudo tcpdump "tcp[tcpflags] == tcp-syn"
```

This is how you hunt for port scans, connection attempts, and anomalies.

## Combining Filters

Mix multiple conditions with `and`, `or`, `not`:

```bash
# SSH traffic from a specific host
sudo tcpdump host 192.168.1.100 and port 22

# All HTTP except from the web server
sudo tcpdump port 80 and not host 10.0.0.50

# DNS queries (port 53) but exclude responses from the router
sudo tcpdump port 53 and not src host 192.168.1.1

# Capture all traffic EXCEPT SSH
sudo tcpdump "not port 22"
```

## Real-World Examples

**Spy on HTTP traffic:**
```bash
sudo tcpdump -i eth0 -A -s 0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

(This is complex but captures HTTP requests/responses with payloads)

**Simpler: Just grab DNS queries:**
```bash
sudo tcpdump -i eth0 -n port 53
```

**Find all SYN packets (connection attempts):**
```bash
sudo tcpdump -i eth0 "tcp[tcpflags] & tcp-syn != 0"
```

**Capture traffic to/from a specific IP, save to file:**
```bash
sudo tcpdump host 192.168.1.50 -w suspect.pcap
```

Then open `suspect.pcap` in Wireshark for detailed analysis.

## Quick Reference

| Command | What it does |
|---------|-------------|
| `tcpdump -i eth0` | Capture on eth0 |
| `tcpdump -i eth0 -w file.pcap` | Save to file |
| `tcpdump -r file.pcap` | Read from file |
| `tcpdump -c 100` | Capture 100 packets, stop |
| `tcpdump host 1.2.3.4` | Filter by IP |
| `tcpdump port 22` | Filter by port |
| `tcpdump tcp` | Filter by protocol |
| `tcpdump -A` | Show as ASCII |
| `tcpdump -X` | Show as hex + ASCII |
| `tcpdump -e` | Show MAC addresses |
| `tcpdump -n` | Don't resolve hostnames |


---

**Pro tip:** Learn the basic filters first. Once you're comfortable, check the man page for advanced options:

```bash
man tcpdump
man pcap-filter
```
