# SIEM, Firewall & IDS Fundamentals

## SIEM (Security Information and Event Management)

### Log Sources

**Host-Centric**
| Example | What it tells you |
|---------|------------------|
| User accessing a file | Who touched what |
| Authentication attempt | Login successes / failures |
| Process execution | What programs ran |
| Registry key add/edit/delete | Persistence, config tampering |
| PowerShell execution | Scripted activity (attackers love this) |

**Network-Centric**
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
| No centralisation | Logs stay on the machine that made them |
| Limited context | One log rarely tells the full story without correlation |
| Limited analysis | Humans can't manually read every log |
| Format issues | Every source logs in a different format |

---

### Core SIEM Features

**Centralised Log Collection** - Pulls logs from all sources into one place via agents or APIs.

**Normalisation**
- Parsing - breaking a log into individual fields
- Normalisation - converting all logs into one consistent format

**Correlation** - Finds relationships between logs from different sources.

Example (all within 5 minutes):
```
1. Haris logs in via VPN from an unknown IP
2. Haris accesses documents on a shared drive
3. Haris executes a PowerShell script
4. System makes an outbound network connection
```
Result: compromised credentials → data access → script execution → possible exfiltration / C2

**Real-Time Alerting** - Rules define conditions that trigger alerts. Alert fires → analyst investigates.

**Dashboards** - Alert highlights, failed login attempts, events ingested, rules triggered, top domains visited.

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
| Agent / Forwarder | Lightweight tool on endpoint that sends logs to SIEM |
| Syslog | Protocol for sending real-time data to a central destination |
| Manual Upload | Ingest offline data for quick analysis |
| Port-Forwarding | SIEM listens on a port, endpoints forward data to it |

---

### Detection Rules

**Event Log Cleared (Event ID 104)**
```
IF LogSource = WinEventLog
AND EventID = 104
THEN Trigger Alert → "Event Log Cleared"
```

**whoami Execution (Event ID 4688)**
```
IF LogSource = WinEventLog
AND EventCode = 4688
AND NewProcessName CONTAINS "whoami"
THEN Trigger Alert → "WHOAMI Command Execution DETECTED"
```

| Event ID | Meaning |
|----------|---------|
| `104` | Event log cleared |
| `4688` | New process created |

---

### Alert Investigation
```
Alert fires → Examine events + check rule conditions
      |
False Positive → Tune the rule
True Positive  → Investigate → Contact asset owner
                     |
         Isolate infected host / Block suspicious IP
```

---

## Firewall Fundamentals

### Firewall Types
| Type | OSI Layer | How it works |
|------|-----------|-------------|
| Stateless | Layer 3-4 | Filters by predetermined rules, no connection memory |
| Stateful | Layer 3-4 | Tracks previous connections in a state table |
| Proxy | Layer 7 | Acts as intermediary, inspects packet contents |
| NGFW | Layer 3-7 | Deep packet inspection, IPS, heuristic analysis, SSL/TLS decryption |

### Firewall Rule Components
| Component | Description |
|-----------|-------------|
| Source address | IP originating the traffic |
| Destination address | IP receiving the traffic |
| Port | Port number for the traffic |
| Protocol | TCP, UDP, ICMP |
| Action | Allow, deny, or forward |
| Direction | Inbound or outbound |

### Linux Firewall Utilities
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
| HIDS | Installed on individual hosts - detailed but resource-intensive |
| NIDS | Monitors whole network - centralised view of all detections |

### Detection Modes
| Mode | How it works | Limitation |
|------|-------------|-----------|
| Signature-based | Matches against known attack patterns | Cannot detect zero-days |
| Anomaly-based | Learns baseline, alerts on deviations | High false positive rate |
| Hybrid | Combines both approaches | - |

### Snort Modes
| Mode | What it does | Use case |
|------|-------------|---------|
| Packet sniffer | Reads and displays packets, no analysis | Network troubleshooting |
| Packet logging | Logs traffic as PCAP file | Forensic investigation |
| NIDS mode | Monitors in real-time, generates alerts | Active threat detection |

### Snort Rule Structure
```
alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:10001; rev:1;)
```
| Component | Description |
|-----------|-------------|
| Action | What to do when rule triggers |
| Protocol | Traffic protocol e.g. `icmp`, `tcp` |
| Source IP | Where traffic originates |
| Source port | Port traffic originates from |
| Destination IP | Where traffic is going |
| Destination port | Port traffic arrives at |
| `msg` | Message shown when alert fires |
| `sid` | Unique signature ID |
| `rev` | Revision number |

---

## Vulnerability Scanner Overview

### Authenticated vs. Unauthenticated Scans
| | Authenticated | Unauthenticated |
|-|--------------|----------------|
| Credentials required | Yes | No - IP address only |
| Identifies | Vulnerabilities exploitable with host access | Vulnerabilities exploitable externally |
| Visibility | Deep - configuration and installed apps | Surface level |
| Example | Scanning internal database with credentials | Scanning a public-facing website |

### Internal vs. External Scans
| | Internal | External |
|-|----------|---------|
| Conducted from | Inside the network | Outside the network |
| Focuses on | Vulnerabilities exploitable inside network | Vulnerabilities exposed to outside attackers |

---

## CVE and CVSS

**CVE (Common Vulnerabilities and Exposures)** - A unique identifier assigned to each discovered vulnerability, developed by MITRE Corporation. Published in a CVE database so people can apply protective measures.

**CVSS (Common Vulnerability Scoring System)** - A score from 0-10 indicating severity, calculated using factors like impact and ease of exploitability.

| CVSS Score | Severity |
|------------|---------|
| 0.0 - 3.9 | Low |
| 4.0 - 6.9 | Medium |
| 7.0 - 8.9 | High |
| 9.0 - 10.0 | Critical |
