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

#Tcpdump: The Basics
This room introduces some basic command-line arguments for using Tcpdump. The Tcpdump tool and its libpcap library are written in C and C++ and were released for Unix-like systems in the late 1980s or early 1990s. Consequently, they are very stable and offer optimal speed. The libpcap library is the foundation for various other networking tools today. Moreover, it was ported to MS Windows as winpcap.
Specify the Network Interface
The first thing to decide is which network interface to listen to using -i INTERFACE. You can choose to listen on all available interfaces using -i any; alternatively, you can specify an interface you want to listen on, such as -i eth0.

A command such as ip address show (or merely ip a s) would list the available network interfaces. In the terminal below, we see one network card, ens5, in addition to the loopback address.


Save the Captured Packets
In many cases, you should check the captured packets again later. This can be achieved by saving to a file using -w FILE. The file extension is most commonly set to .pcap. The saved packets can be inspected later using another program, such as Wireshark. You won’t see the packets scrolling when you choose the -w option.

Read Captured Packets from a File
You can use Tcpdump to read packets from a file by using -r FILE. This is very useful for learning about protocol behaviour. You can capture network traffic over a suitable time frame to inspect a specific protocol, then read the captured file while applying filters to display the packets you are interested in. Furthermore, it might be a packet capture file that contains a network attack that took place, and you inspect it to analyze the attack.

Limit the Number of Captured Packets
You can specify the number of packets to capture by specifying the count using -c COUNT. Without specifying a count, the packet capture will continue till you interrupt it, for example, by pressing CTRL-C. Depending on your goal, you only need a limited number of packets.

Command	Explanation
tcpdump -i INTERFACE	Captures packets on a specific network interface
tcpdump -w FILE	Writes captured packets to a file
tcpdump -r FILE	Reads captured packets from a file
tcpdump -c COUNT	Captures a specific number of packets
tcpdump -n	Don’t resolve IP addresses
tcpdump -nn	Don’t resolve IP addresses and don’t resolve protocol numbers
tcpdump -v	Verbose display; verbosity can be increased with -vv and -vvv

Filtering by Host
Let’s say you are only interested in IP packets exchanged with your network printer or a specific game server. You can easily limit the captured packets to this host using host IP or host HOSTNAME. In the terminal below, we capture all the packets exchanged with example.com and save them to http.pcap. It is important to note that capturing packets requires you to be logged-in as root or to use sudo.


Terminal
user@TryHackMe$ sudo tcpdump host example.com -w http.pcap
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
16:49:02.482295 IP 192.168.139.132.49480 > 93.184.215.14.http: Flags [S], seq 3330895816, win 32120, options [mss 1460,sackOK,TS val 621343956 ecr 0,nop,wscale 7], length 0
16:49:02.635087 IP 93.184.215.14.http > 192.168.139.132.49480: Flags [S.], seq 2231582859, ack 3330895817, win 64240, options [mss 1460], length 0
16:49:02.635125 IP 192.168.139.132.49480 > 93.184.215.14.http: Flags [.], ack 1, win 32120, length 0
16:49:02.635491 IP 192.168.139.132.49480 > 93.184.215.14.http: Flags [P.], seq 1:131, ack 1, win 32120, length 130: HTTP: GET / HTTP/1.1
16:49:02.635580 IP 93.184.215.14.http > 192.168.139.132.49480: Flags [.], ack 131, win 64240, length 0
[...]
^C
13 packets captured
25 packets received by filter
0 packets dropped by kernel
If you want to limit the packets to those from a particular source IP address or hostname, you must use src host IP or src host HOSTNAME. Similarly, you can limit packets to those sent to a specific destination using dst host IP or dst host HOSTNAME.

Filtering by Port
If you want to capture all DNS traffic, you can limit the captured packets to those on port 53. Remember that DNS uses UDP and TCP ports 53 by default. In the following example, we can see all the DNS queries read by our network card. The terminal below shows two DNS queries: the first query requests the IPv4 address used by example.org, while the second requests the IPv6 address associated with example.org.


Terminal
user@TryHackMe$ sudo tcpdump -i ens5 port 53 -n
[sudo] password for strategos: 
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
17:26:33.591670 IP 192.168.139.132.47902 > 192.168.139.2.53: 47108+ A? example.org. (29)
17:26:33.591717 IP 192.168.139.132.47902 > 192.168.139.2.53: 5+ AAAA? example.org. (29)
17:26:33.593324 IP 192.168.139.2.53 > 192.168.139.132.47902: 47108 1/0/0 A 93.184.215.14 (45)
17:26:33.593325 IP 192.168.139.2.53 > 192.168.139.132.47902: 5 1/0/0 AAAA 2606:2800:21f:cb07:6820:80da:af6b:8b2c (57)
[...]
^C
12 packets captured
12 packets received by filter
0 packets dropped by kernel
In the above example, we captured all the packets sent to or from a specific port number. You can limit the packets to those from a particular source port number or to a particular destination port number using src port PORT_NUMBER and dst port PORT_NUMBER, respectively.

Filtering by Protocol
The final type of filtering we will cover is filtering by protocol. You can limit your packet capture to a specific protocol; examples include: ip, ip6, udp, tcp, and icmp. In the example below, we limit our packet capture to ICMP packets. We can see an ICMP echo request and reply, which is a possible indication that someone is running the ping command. There is also an ICMP time exceeded; this might be due to running the traceroute command (as explained in the Networking Essentials room).


Terminal
user@TryHackMe$ sudo tcpdump -i ens5 icmp -n
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
18:11:00.624681 IP 192.168.139.132 > 93.184.215.14: ICMP echo request, id 47038, seq 1, length 64
18:11:00.781482 IP 93.184.215.14 > 192.168.139.132: ICMP echo reply, id 47038, seq 1, length 64
18:11:04.168792 IP 192.168.139.2 > 192.168.139.132: ICMP time exceeded in-transit, length 68
18:11:04.168815 IP 192.168.139.2 > 192.168.139.132: ICMP time exceeded in-transit, length 68
[...]
18:11:14.857188 IP 93.184.215.14 > 192.168.139.132: ICMP 93.184.215.14 udp port 33495 unreachable, length 68
^C
52 packets captured
52 packets received by filter
0 packets dropped by kernel
Command	Explanation
tcpdump host IP or tcpdump host HOSTNAME	Filters packets by IP address or hostname
tcpdump src host IP or	Filters packets by a specific source host
tcpdump dst host IP	Filters packets by a specific destination host
tcpdump port PORT_NUMBER	Filters packets by port number
tcpdump src port PORT_NUMBER	Filters packets by the specified source port number
tcpdump dst port PORT_NUMBER	Filters packets by the specified destination port number
tcpdump PROTOCOL	Filters packets by protocol; examples include ip, ip6, and icmp
