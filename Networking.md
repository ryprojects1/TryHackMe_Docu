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

We used the username anonymous to log in
We didn’t need to provide any password
Issuing ls returned a list of files available for download
type ascii switched to ASCII mode as this is a text file
get coffee.txt allowed us to retrieve the file we want

Let’s present some of the commands used by your mail client when it transfers an email to an SMTP server:

HELO or EHLO initiates an SMTP session
MAIL FROM specifies the sender’s email address
RCPT TO specifies the recipient’s email address
DATA indicates that the client will begin sending the content of the email message
. is sent on a line by itself to indicate the end of the email message

Some common POP3 commands are:

USER <username> identifies the user
PASS <password> provides the user’s password
STAT requests the number of messages and total size
LIST lists all messages and their sizes
RETR <message_number> retrieves the specified message
DELE <message_number> marks a message for deletion
QUIT ends the POP3 session applying changes, such as deletions

IMAP allows synchronizing read, moved, and deleted messages. IMAP is quite convenient when you check your email via multiple clients. Unlike POP3, which tends to minimize server storage as email is downloaded and deleted from the remote server, IMAP tends to use more storage as email is kept on the server and synchronized across the email clients.

The IMAP protocol commands are more complicated than the POP3 protocol commands. We list a few examples below:

LOGIN <username> <password> authenticates the user
SELECT <mailbox> selects the mailbox folder to work with
FETCH <mail_number> <data_item_name> Example fetch 3 body[] to fetch message number 3, header and body.
MOVE <sequence_set> <mailbox> moves the specified messages to another mailbox
COPY <sequence_set> <data_item_name> copies the specified messages to another mailbox
LOGOUT logs out
