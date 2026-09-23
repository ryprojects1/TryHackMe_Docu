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

#Wireshark: The Basics
Wireshark is an open-source, cross-platform network packet analyser tool capable of sniffing and investigating live traffic and inspecting packet captures (PCAP). It is commonly used as one of the best packet analysis tools. In this room, we will look at the basics of Wireshark and use it to perform fundamental packet analysis.
Wireshark is one of the most potent traffic analyser tools available in the wild. There are multiple purposes for its use:

Detecting and troubleshooting network problems, such as network load failure points and congestion.
Detecting security anomalies, such as rogue hosts, abnormal port usage, and suspicious traffic.
Investigating and learning protocol details, such as response codes and payload data.
Wireshark is one of the most potent traffic analyser tools available in the wild. There are multiple purposes for its use:

Toolbar	The main toolbar contains multiple menus and shortcuts for packet sniffing and processing, including filtering, sorting, summarising, exporting and merging. 
Display Filter Bar	The main query and filtering section.
Recent Files	List of the recently investigated files. You can recall listed files with a double-click. 
Capture Filter and Interfaces	Capture filters and available sniffing points (network interfaces).  The network interface is the connection point between a computer and a network. The software connection (e.g., lo, eth0 and ens33) enables networking hardware.
Status Bar	Tool status, profile and numeric packet information.

Packet List Pane	Summary of each packet (source and destination addresses, protocol, and packet info). You can click on the list to choose a packet for further investigation. Once you select a packet, the details will appear in the other panels.
Packet Details Panel	Detailed protocol breakdown of the selected packet.
Packet Bytes Pane	Hex and decoded ASCII representation of the selected packet. It highlights the packet field depending on the clicked section in the details pane.

Packet Dissection
Packet dissection is also known as protocol dissection, which investigates packet details by decoding available protocols and fields. Wireshark supports a long list of protocols for dissection, and you can also write your dissection scripts. You can find more details on dissection

Go to Packet
Packet numbers do not only help to count the total number of packets or make it easier to find/investigate specific packets. This feature not only navigates between packets up and down; it also provides in-frame packet tracking and finds the next packet in the particular part of the conversation. You can use the "Go" menu and toolbar to view specific packets.

Find Packets
Apart from packet number, Wireshark can find packets by packet content. You can use the "Edit --> Find Packet" menu to make a search inside the packets for a particular event of interest. This helps analysts and administrators to find specific intrusion patterns or failure traces.

There are two crucial points in finding packets. The first is knowing the input type. This functionality accepts four types of inputs (Display filter, Hex, String and Regex). String and regex searches are the most commonly used search types. Searches are case insensitive, but you can set the case sensitivity in your search by clicking the radio button.

The second point is choosing the search field. You can conduct searches in the three panes (packet list, packet details, and packet bytes), and it is important to know the available information in each pane to find the event of interest. For example, if you try to find the information available in the packet details pane and conduct the search in the packet list pane, Wireshark won't find it even if it exists.

