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

# CAPA: The Basics
CAPA (Common Analysis Platform for Artifacts) is a tool developed by the FireEye Mandiant team. It is designed to identify the capabilities present in executable files like Portable Executables (PE), ELF binaries, .NET modules, shellcode, and even sandbox reports. It does so by analyzing the file and applying a set of rules that describe common behaviours, allowing it to determine what the program is capable of doing, such as network communication, file manipulation, process injection, and many more.

Option	Description	Sample Syntax
-h or --help

Show this help message and exit.
capa -h

-v or --verbose	Enable verbose result document.	capa.exe .\cryptbot.bin -v
-vv or --vverbose	Enable a very verbose result document.	capa.exe .\cryptbot.bin -vv
The first block contains basic information about the file. This includes the following:

The cryptographic algorithms, such as the md5, and sha1/256.
The analysis field tells us how CAPA performed its analysis on the file.
The os field reveals the operating system (OS) context for which the identified capabilities apply.
The arch field allows us to determine whether we are dealing with a binary related to x86 architecture.
The path where the analyzed file was located.


Objective	Explanation
Anti-Behavioral Analysis	Malware attempts to avoid detection by hindering behavioural analysis using tools like sandboxes or debuggers.
Anti-Static Analysis	Malware attempts to obstruct or add complexity to static analysis, making it more challenging for security professionals to identify and understand its malicious behaviours and intentions.
Collection	Malware focuses on identifying and gathering information from the targeted machine or network.
Command and Control	Malware typically establishes communication with compromised systems through various methods such as command and control servers, peer-to-peer networks, or other means. This communication allows the malware to control the compromised systems, enabling the attackers to execute commands, exfiltrate data, or carry out other malicious activities.
Credential Access	The primary aim of malware is to steal account credentials, such as usernames and passwords.
Defense Evasion	The malware aims to bypass and circumvent the various detection and security mechanisms present within the system to avoid being detected or mitigated.
Discovery	Malware Seeks to collect detailed information about the configuration and setup of the system or network environment, including hardware, software, and network infrastructure.
Execution	Malware is designed to execute unauthorized commands or code on a targeted computer system without the user’s consent. This can include a wide range of harmful activities, such as stealing personal information, damaging files, or gaining unauthorized access to the system.
Exfiltration	Malware is designed to infiltrate computer systems or networks to steal and extract sensitive data. This can include personal information, financial details, and any other valuable data stored on the targeted system or network.
Impact	Malware aims to manipulate, disrupt, or damage computer systems and data. It can enter computers through infected emails, compromised websites, and other deceptive means, leading to financial loss, privacy breaches, and system instability.
Lateral Movement	Malware seeks to spread through the network, either actively through machine access or passively, such as via malicious emails.
Persistence	Malware is intentionally developed with the capability to remain undetected and operational on a computer system for an extended period.
Privilege Escalation	Malware seeks to infiltrate a computer system or network to gain elevated permissions or control. Once inside the target environment, malware can seek to escalate its privileges, access sensitive information, or take control of system resources for malicious purposes.
Micro-Objective	Description
PROCESS	exhibiting behaviours related to processes such as but not limited to Creating Process, Setting Thread Context, Terminating Process, and Checking Mutex.
MEMORY	exhibiting behaviours such as, but not limited to, Allocating Memory, Changing Memory Protection, and Freeing Memory.
COMMUNICATION	exhibiting behaviours such as (not limited to (DNS, FTP, HTTP, ICMP, SMTP) network traffic.
DATA	exhibiting behaviours such as but not limited to Checking strings, compressing, decoding and encoding data
Objective	Behavior	Identifiers	Explanation
ANTI-BEHAVIORAL ANALYSIS	Lab Machine Detection	B0009	The malware checks to see if it is running in a virtual environment. During its system reconnaissance, the malware examines various user and system artifacts.
ANTI-STATIC ANALYSIS	Executable Code Obfuscation	B0032	Executable code has been intentionally obscured to prevent static code analysis. This is a specific behavior related to the executable code of a malware sample, including its data and text sections.
EXECUTION	Command and Scripting Interpreter	E1059	Malware can exploit command and script interpreters to run malicious commands, scripts, or binaries. It targets built-in interpreters like cmd.exe or PowerShell on Windows or Bash on Unix-like systems. Attackers may also use other scripting languages like Python, Perl, or JavaScript.
DISCOVERY	File and Directory Discovery	E1083	Malware has the capability to search for specific files in particular locations by enumerating files and directories.
ANTI-STATIC ANALYSIS, DEFENSE EVASION	Obfuscated Files or Information	E1027	Malware can obfuscate files or information by encoding, encrypting, or otherwise, making them hard to analyze. It can also encode or encrypt malware samples itself.
CAPA uses namespaces to group items with the same purpose. 
Top-Level Namespace (TLN)	Explanation
anti-analysis	contains a set of rules specifically designed to detect behaviours exhibited by malware to evade analysis. These behaviours include obfuscation, packing, and anti-debugging techniques.
collection	contains a set of data-related rules that malware may enumerate and collect for exfiltration or other purposes. Think of it as the “data-gathering” aspect of malware behaviour.
communication	contains a set of rules that pertain to different communication behaviours demonstrated by malware. This encompasses how malware interacts with networks, including data transmission and reception, command and control communications, and other network-related behaviours.
compiler	contains a set of rules and configurations for recognizing specific build environments or compilers employed in generating executables. These namespaces essentially serve as the unique “signature” that identifies the compilation process of a program.
data-manipulation	contains a set of rules that govern the behaviours involved in altering data within executable files. This aspect can be considered the “data transformation” component of malware behaviour, encompassing actions such as String Encryption and Data Encoding.
View More Tables
executable	contains a set of rules pertaining to the attributes in executable files. These attributes include PE sections or debug info associated with the executable.
host-interaction	contains a set of rules related to behaviors involving interactions with the host system. This encompasses how malware interacts with its environment. Specifically, the rules in this namespace may capture behaviors related to reading, writing, or modifying files on disk, including creating, deleting, or modifying files and directories.”
impact	contains a set of rules related to the potential consequences or effects of a program’s behavior. Think of it as the aspect that focuses on the possible harm that this malware can cause. It may include behaviors related to establishing remote access, data exfiltration, destruction, or modification.”
internal	Rules contained within the system are not intended for direct use by analysts or for reporting. Instead, these rules are meant for internal purposes within the CAPA tool, serving as the behind-the-scenes aspect of rule development and execution.
lib	building blocks to create other rules
linking	contains rules to identify behaviors involving linking or dynamically loading external code or libraries during program execution. This is its primary function and is crucial for the program’s security. Understanding linking behavior is essential for several reasons. Malware often depends on external libraries or components (such as OpenSSL, Zlib, or other third-party libraries) to carry out specific tasks. Detecting these dependencies helps analysts understand the malware’s capabilities. External libraries also introduce an additional attack surface. If a vulnerability exists in a linked library, it can be exploited by the malware or defenders during analysis.
load-code	contains a set of rules and regulations related to the behaviors associated with dynamically loading or executing code during program execution. This concept can be equated to the “runtime code injection” aspect of malware behavior, which involves unauthorized code introduction during a program’s execution.
malware-family	contains a set of rules related to behaviors that are linked to particular malware families or groups. It serves as a way to identify the distinct characteristics or “signatures” associated with known malware families, allowing for more accurate detection and classification of potential threats.
nursery	this is a staging ground that contains rules for those who are not quite polished
persistence	contains rules related to behaviors associated with maintaining access or persistence within a compromised system. This namespace is essentially focused on understanding how malware can establish and maintain a presence within a compromised environment, allowing it to persist and carry out malicious activities over an extended period.
runtime	contains a set of rules that seek to identify the language or platform on which the program runs.
targeting	contains a set of rules related to behaviors exhibited by programs that interact with ATMs.
Top-Level Namespace (TLN)	Namespaces	Rule YAML File	Explanation
Anti-Analysis	anti-vm/vm-detection	reference-anti-vm-strings-targeting-virtualbox.yml

reference-anti-vm-strings-targeting-virtualpc.yml
“anti-vm/vm-detection” namespace contains rules to detect lab machine (VM) environments. These rules focus on identifying specific strings or patterns commonly used by malware to detect VMs while running. Using these rules, CAPA can identify if malware searches for VMware-specific registry keys, the presence of VMware tools, or other VM-related elements.

obfuscation	obfuscated-with-dotfuscator.yml

obfuscated-with-smartassembly.yml
Malware often uses obfuscation techniques to make analysis more difficult. These include methods such as String Encryption, Code Obfuscation, Packing, and Anti-Debugging Tricks. The obfuscation namespace addresses these techniques, which conceal or obscure the true purpose of the code.
