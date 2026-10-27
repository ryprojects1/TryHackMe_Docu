# SIEM, Firewall & IDS Fundamentals

---

## SIEM (Security Information and Event Management)

### Log Sources

**Host-Centric** — events that happen on a host

| Example | What it tells you |
|---------|------------------|
| User accessing a file | Who touched what |
| Authentication attempt | Login successes / failures |
| Process execution | What programs ran |
| Registry key add/edit/delete | Persistence, config tampering |
| PowerShell execution | Scripted activity (attackers love this) |

**Network-Centric** — generated when hosts communicate

| Example |
|---------|
| SSH connections |
| File access via FTP |
| Web traffic |
| VPN access |
| Network file sharing activity |

---

### Why Manual Log Analysis Doesn't Scale

| Problem | Why it hurts |
|---------|-------------|
| Numerous log sources | Hundreds of events per second across loads of devices |
| No centralisation | Logs stay on the machine that made them — slow to investigate |
| Limited context | One log rarely tells the full story without correlation |
| Limited analysis | Humans can't manually read every log — stuff gets missed |
| Format issues | Every source logs in a different format |

---

### Core SIEM Features

**Centralised Log Collection**
Pulls logs from all sources into one place via agents or APIs.

**Normalisation**
- Parsing — breaking a log into individual fields
- Normalisation — converting all logs into one consistent format

**Correlation**
Finds relationships between logs from different sources.

Example (all within 5 minutes):
```
1. Haris logs in via VPN from an unknown IP
2. Haris accesses documents on a shared drive
3. Haris executes a PowerShell script
4. System makes an outbound network connection
```
Individually each looks fine — together it looks like:
**compromised credentials → data access → script execution → possible exfiltration / C2**

**Real-Time Alerting**
- Rules define conditions that trigger alerts
- Default rules included, analysts write custom ones over time
- Alert fires → analyst investigates inside the SIEM

**Dashboards**
The most important part of a SIEM. Shows:
- Alert highlights
- Failed login attempts
- Events ingested count
- Rules triggered
- Top domains visited

---

### Log Locations on Linux

| Path | Contents |
|------|---------|
| `/var/log/httpd` | HTTP request/response and error logs |
| `/var/log/cron` | Cron job events |
| `/var/log/auth.log` / `/var/log/secure` | Authentication logs |
| `/var/log/kern` | Kernel events |

---

### Log Ingestion Methods

| Method | How it works |
|--------|-------------|
| Agent / Forwarder | Lightweight tool on endpoint that captures and sends logs to SIEM |
| Syslog | Widely used protocol for sending real-time data to a central destination |
| Manual Upload | Ingest offline data for quick analysis |
| Port-Forwarding | SIEM listens on a port, endpoints forward data to it |

---

### Detection Rules

Rules are **IF [conditions] THEN [alert]**

**Use Case 1 — Event Log Cleared**
Attackers clear logs post-exploitation to cover tracks. Windows logs Event ID 104.
```
IF LogSource = WinEventLog
AND EventID = 104
THEN Trigger Alert → "Event Log Cleared"
```

**Use Case 2 — whoami Execution**
Attackers run whoami after exploitation to check their privileges.
```
IF LogSource = WinEventLog
AND EventCode = 4688
AND NewProcessName CONTAINS "whoami"
THEN Trigger Alert → "WHOAMI Command Execution DETECTED"
```

**Key Event IDs**

| Event ID | Meaning |
|----------|---------|
| `104` | Event log cleared |
| `4688` | New process created |

---

### Alert Investigation

```
Alert fires
↓
Examine events + check which rule conditions were met
↓
┌──────────────────────────────┐
▼                              ▼
False Positive            True Positive
↓                              ↓
Tune the rule             Investigate further
                               ↓
                        Contact asset owner
                               ↓
                  ┌────────────────────────┐
                  ▼                        ▼
         Isolate infected host    Block suspicious IP
```

---

## Firewall Fundamentals

Inspects incoming and outgoing network traffic — like a security guard deciding who gets in.

### Firewall Types

| Type | OSI Layer | How it works |
|------|-----------|-------------|
| **Stateless** | Layer 3–4 | Filters packets by predetermined rules, no memory of previous connections. Fast but limited. |
| **Stateful** | Layer 3–4 | Tracks previous connections in a state table. More secure than stateless. |
| **Proxy** | Layer 7 | Acts as intermediary, inspects packet contents, masks internal IPs |
| **NGFW** | Layer 3–7 | Deep packet inspection, built-in IPS, heuristic analysis, SSL/TLS decryption |

---

### Firewall Rule Components

| Component | Description |
|-----------|-------------|
| Source address | IP originating the traffic |
| Destination address | IP receiving the traffic |
| Port | Port number for the traffic |
| Protocol | Protocol used (TCP, UDP, ICMP) |
| Action | Allow, deny, or forward |
| Direction | Inbound or outbound |

### Rule Types

- **Inbound** — applied to incoming traffic e.g. allow HTTP on port 80
- **Outbound** — applied to outgoing traffic e.g. block SMTP port 25
- **Forward** — forward specific traffic inside the network e.g. HTTP to web server

---

### Linux Firewall Utilities (all use Netfilter framework)

| Tool | Description |
|------|-------------|
| `iptables` | Most widely used, complex syntax |
| `nftables` | Successor to iptables, enhanced capabilities |
| `firewalld` | Predefined rule sets, zone-based |
| `ufw` | Beginner-friendly wrapper around iptables |

---

## IDS Fundamentals

### Deployment Modes

| Mode | Description |
|------|-------------|
| **HIDS** | Installed on individual hosts — detailed visibility but resource-intensive |
| **NIDS** | Monitors whole network traffic — centralised view of all detections |

### Detection Modes

| Mode | How it works | Limitation |
|------|-------------|-----------|
| **Signature-based** | Matches traffic against known attack patterns (signatures) | Cannot detect zero-day attacks |
| **Anomaly-based** | Learns normal baseline behaviour, alerts on deviations | High false positive rate |
| **Hybrid** | Combines both — uses signatures for known threats, anomaly for new ones | — |

---

### Snort Modes

| Mode | What it does | Use case |
|------|-------------|---------|
| Packet sniffer | Reads and displays packets, no analysis | Network troubleshooting |
| Packet logging | Logs traffic as PCAP file | Forensic investigation |
| NIDS mode | Monitors in real-time, applies rules, generates alerts | Active threat detection |

---

### Snort Rule Structure

```
alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:10001; rev:1;)
```

| Component | Description |
|-----------|-------------|
| Action | What to do when rule triggers e.g. `alert` |
| Protocol | Traffic protocol e.g. `icmp`, `tcp` |
| Source IP | Where traffic originates — `any` for all |
| Source port | Port traffic originates from — `any` for all |
| Destination IP | Where traffic is going — `$HOME_NET` for your network |
| Destination port | Port traffic arrives at — `any` for all |
| `msg` | Message shown when alert fires |
| `sid` | Unique signature ID for the rule |
| `rev` | Revision number — increments each time rule is modified |

# Vulnerability Scanner Overview
Authenticated vs. Unauthenticated Scans
Authenticated scans require the subject host's credentials and are more detailed than unauthenticated scans. These types of scans are helpful for discovering the threat surface within the host. However, unauthenticated scans are conducted without providing any credentials of the subject host. These scans help identify the threat surface from outside the host.

Authenticated Scans	Unauthenticated Scans
The credentials of the subject host must be provided to the vulnerability scanner.	The vulnerability scanner does not require the host’s credentials; it only needs the IP address.
Identifies the vulnerabilities that can be exploited by the attackers having access to the host.	Identifies the vulnerabilities that can be exploited by an external attacker having no access to the subject host.
It provides a deeper visibility into the target system by scanning its configuration and installed applications.	It is less resource-intensive and straightforward to set up.
For example, scanning an internal database by providing its credentials to the vulnerability scanner.	For example, scanning a public-facing website for vulnerabilities that any user can exploit.
Internal vs. External Scans
Internal scans are conducted from inside the network, while external scans are conducted from outside the network. Let's see a few of their differences below.

Internal Scans	External Scans
Conducted from inside the network.	Conducted from outside the network.
It focuses on the vulnerabilities that can be exploited inside the network.	It focuses on the vulnerabilities that can be exploited from outside the network.
Identifies vulnerabilities that would be exposed to the attackers once they get inside the network.	Identifies the vulnerabilities exposed to the attacker from outside the network.
CVE
CVE stands for Common Vulnerabilities and Exposures. Consider CVE a unique number for each of your inquiries and complaints. If there is any update to any issue, you can easily follow up on that using the unique CVE number. Coming out of the help desk example scenario, this CVE number is a unique number given to vulnerabilities. This was developed by the MITRE Corporation. Whenever a new vulnerability is discovered in any software application, it is given a unique CVE number as a reference and published online in a CVE database. This publication aims to make people aware of these vulnerabilities so they can apply protective measures to remediate them. You can find the details of any previously discovered vulnerability in the CVE database. 
CVSS
CVSS stands for Common Vulnerability Scoring System. If we return to the help desk example again, you would always need to prioritize the complaints. The most efficient way to prioritize the complaints is by their severity level. What if all your complaints are registered with a score ranging from 0 to 10, where a higher score indicates a more severe complaint? This would resolve the problem of prioritizing critical complaints. This is called a CVSS score. In the computing world, just as each vulnerability has a CVE number that uniquely identifies it, each has a CVSS score that tells you its severity. The CVSS score is calculated by considering multiple factors, including its impact, ease of exploitability, etc. 
CVSS Score Range	Severity Levels
0.0-3.9	Low
4.0-6.9	Medium
7.0-8.9	High
9.0-10	Critical
