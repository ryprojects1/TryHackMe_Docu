# 🛡️ Defensive Security, SOC & Digital Forensics

> TryHackMe notes. Covers what a SOC actually does, who does what, the main tools, the basics of digital forensics, and then incident response + logs.

---

## 📑 Contents

- [Part 1: Defensive Security & SOC](#part-1-defensive-security--soc)
  - [Core SOC Responsibilities](#core-soc-responsibilities)
  - [SOC Team Structure](#soc-team-structure)
  - [Incident Report Example](#incident-report-example)
  - [Key SOC Tools](#key-soc-tools)
  - [Tool Comparison](#tool-comparison)
- [Part 2: Digital Forensics Fundamentals](#part-2-digital-forensics-fundamentals)
  - [NIST's 4 Phases](#nists-4-phases-of-digital-forensics)
  - [Types of Digital Forensics](#types-of-digital-forensics)
  - [Authorization](#authorization)
  - [Chain of Custody](#chain-of-custody)
  - [Write Blockers](#write-blockers)
  - [Forensic Images](#forensic-images-windows)
  - [Metadata Tools](#metadata-tools)
- [Part 3: Incident Response Fundamentals](#part-3-incident-response-fundamentals)
  - [True vs False Positives](#true-vs-false-positives)
  - [Types of Incidents](#types-of-incidents)
  - [SANS Framework (PICERL)](#sans-incident-response-framework-picerl)
  - [Incident Response Plan](#incident-response-plan)
  - [IR Tools](#incident-response-tools)
  - [Playbooks vs Runbooks](#playbooks-vs-runbooks)
- [Part 4: Logs](#part-4-logs)
  - [Why Logs Matter](#why-logs-matter)
  - [Log Types](#log-types)
  - [Windows Event IDs](#important-windows-event-ids)
  - [Reading a Web Server Log](#reading-a-web-server-access-log)
- [Key Takeaways](#-key-takeaways)

---

# Part 1: Defensive Security & SOC

A **SOC (Security Operations Centre)** is the team that watches an organisation's systems 24/7, spots threats, and responds to them. It does two main things: **detection** and **response**.

## Core SOC Responsibilities

### 🔍 Detection

| What to Detect | Description |
|---|---|
| **Vulnerabilities** | Weaknesses in software/OS that attackers can exploit |
| **Unauthorised activity** | e.g. an attacker using stolen credentials to log in |
| **Policy violations** | e.g. downloading pirated software, sending files insecurely |
| **Intrusions** | Unauthorised access to systems or networks |

### 🚨 Response

- Minimise the impact of detected incidents
- Perform **root cause analysis** (work out *why* it happened, not just *what*)
- Support the incident response team

---

## SOC Team Structure

| Role | Responsibilities |
|---|---|
| **SOC Analyst L1** | First responder: basic alert triage, decides if a detection is harmful, reports through proper channels |
| **SOC Analyst L2** | Deeper investigation: correlates data from multiple sources |
| **SOC Analyst L3** | Experienced: proactively hunts threats, leads containment, eradication and recovery |
| **Security Engineer** | Deploys and configures security solutions |
| **Detection Engineer** | Writes the detection rules/logic behind security tools |
| **SOC Manager** | Manages SOC processes, reports to the CISO on security posture |

> 💡 L1 → L2 → L3 is basically an escalation ladder. L1 filters the noise, L2 digs in, L3 hunts and leads the fix.

---

## Incident Report Example

A good incident report answers the **5 Ws**:

| Field | Detail |
|---|---|
| **What** | Malicious file detected on a host |
| **When** | 13:20, June 5 2024 |
| **Where** | Directory on "GEORGE PC" |
| **Who** | User: George |
| **Why** | Downloaded from a pirated software website to use software for free |

---

## Key SOC Tools

### 📊 SIEM (Security Information and Event Management)

- Collects logs from multiple network devices (**log sources**)
- Provides **detection** capabilities
- Used in almost every SOC environment

### 💻 EDR (Endpoint Detection and Response)

- Real-time **and** historical visibility of device activity
- Operates at the **endpoint** level
- Can carry out **automated responses**
- Detailed investigation and response with a few clicks

### 🧱 Firewall

- Barrier between internal and external networks
- Monitors and filters incoming/outgoing traffic
- Blocks suspicious traffic **before** it reaches the internal network
- Operates at the **network** level

---

## Tool Comparison

| Tool | Level | Main Function | Can Respond? |
|---|---|---|---|
| SIEM | Network | Log collection + detection | ❌ Detection only |
| EDR | Endpoint | Real-time visibility + detection | ✅ Automated response |
| Firewall | Network | Traffic filtering + blocking | ✅ Blocks traffic |

> 🔗 **Link to my home lab:** UFW = the firewall layer, Fail2Ban = an IPS that actually responds (bans IPs), and reading `auth.log` / `fail2ban.log` by hand is a tiny, manual version of what a SIEM automates.

---

# Part 2: Digital Forensics Fundamentals

**Forensics** = applying methods and procedures to investigate and solve crimes.

**Digital forensics** = the branch that investigates **cyber crime**, meaning any criminal activity carried out on or using a digital device.

---

## NIST's 4 Phases of Digital Forensics

NIST (National Institute of Standards and Technology) defines the process in four phases:

```
Collection ➜ Examination ➜ Analysis ➜ Reporting
```

| Phase | What happens |
|---|---|
| **1. Collection** | Identify every device data can be collected from (PCs, laptops, cameras, USBs…). Make sure the original data **isn't tampered with**, and document every item collected. |
| **2. Examination** | There's usually way too much data, so filter it down to what matters. e.g. only media from a specific date/time, or only one user's data on a shared system. |
| **3. Analysis** | The critical bit. Correlate multiple pieces of evidence to draw conclusions and build a **chronological timeline** of activity. |
| **4. Reporting** | Write a detailed report covering methodology, findings and recommendations. It goes to law enforcement and execs, so include an **executive summary** for non-technical readers. |

---

## Types of Digital Forensics

| Type | What it covers |
|---|---|
| **Computer forensics** | The most common type: investigating computers, the devices most used in crimes |
| **Mobile forensics** | Call records, texts, GPS locations, etc. from mobile devices |
| **Network forensics** | Goes beyond single devices to the whole network; mostly **network traffic logs** |
| **Database forensics** | Investigating intrusions into databases that led to data **modification or exfiltration** |
| **Cloud forensics** | Investigating data on cloud infrastructure; can be tricky because there's often **little evidence** |
| **Email forensics** | Checking whether emails are part of **phishing or fraud campaigns** |

---

## Authorization

> ⚠️ The forensics team must get **authorisation from the relevant authorities before collecting any data**.

- Evidence collected without approval may be **inadmissible in court**
- Forensic evidence contains private and sensitive data, so authorisation keeps the investigation within the law

---

## Chain of Custody

**The problem:** evidence gets collected, then a few days later something goes missing or gets changed. Without a record, nobody can be held accountable.

**The fix:** a **chain of custody** document that records:

- [ ] Description of the evidence (name, type)
- [ ] Name of who collected it
- [ ] Date and time of collection
- [ ] Storage location of each piece of evidence
- [ ] Access times + who accessed it

---

## Write Blockers

When you plug a suspect's hard drive into a forensic workstation, background tasks on the workstation can **alter file timestamps** on the drive, which messes up the analysis and gives wrong results.

A **write blocker** sits in between and **blocks any action that would alter the evidence**, so the drive stays in its original state.

```
Suspect Drive ──▶ [ WRITE BLOCKER ] ──▶ Forensic Workstation
                   (read ✅ / write ❌)
```

---

## Forensic Images (Windows)

Forensic images are **bit-by-bit copies** of the whole OS. Two types:

| Image | Data | Volatile? | Examples |
|---|---|---|---|
| **Disk image** | Everything on the storage device (HDD/SSD) | ❌ Non-volatile (survives a restart) | Media, documents, browsing history |
| **Memory image** | Everything in RAM | ✅ Volatile (lost on shutdown/restart) | Open files, running processes, current network connections |

> 🧠 **Take the memory image first.** If the machine restarts or shuts down, all the volatile data is gone for good.

---

## Metadata Tools

| Tool | Use |
|---|---|
| `pdfinfo` | Shows PDF metadata: title, subject, author, creator, creation date |
| `exiftool` | Reads **and writes** metadata in loads of file types, e.g. JPEG images |

```bash
# PDF metadata
pdfinfo document.pdf

# Image metadata
exiftool photo.jpg
```

---

# Part 3: Incident Response Fundamentals

## True vs False Positives

Not every alert is an actual attack. Working out which is which is a huge part of L1's job.

| Type | Example |
|---|---|
| ❌ **False positive** | Alert fires on a big data transfer from a system to an external IP. Turns out it was just a scheduled backup to cloud storage. Alert fired, but nothing bad happened. |
| ✅ **True positive** | Alert fires on a phishing attempt against a user. Analysis confirms it really was a phishing email trying to compromise the system. |

> 💡 Too many false positives = alert fatigue, and that's how real attacks get missed.

---

## Types of Incidents

These can happen on their own or all at once to the same victim.

| Incident | What it is |
|---|---|
| **Malware infection** | A malicious program that damages a system, network or app. Most incidents involve malware in some way. |
| **Security breach** | An unauthorised person gets access to confidential data |
| **Data leak** | Confidential info gets exposed to unauthorised people. Often used for reputational damage or to threaten/extort the victim. |
| **Insider attack** | The attack comes from *inside* the organisation, e.g. a disgruntled employee plugging in an infected USB on their last day |
| **Denial of Service (DoS)** | Attacks **availability** (one of the CIA triad), flooding a system so legit users can't use it |

---

## SANS Incident Response Framework (PICERL)

SANS splits incident response into 6 phases. Easy way to remember: **PICERL**.

```
Preparation ➜ Identification ➜ Containment ➜ Eradication ➜ Recovery ➜ Lessons Learned
```

| Phase | What happens | Example |
|---|---|---|
| **P**reparation | Build everything you need *before* an incident: IR team, IR plan, security tools deployed | Phishing awareness training for employees |
| **I**dentification | Look for abnormal behaviour that might be an incident, using monitoring tools | Huge amount of data leaving a host. Analysis shows it was compromised by a malicious attachment from a phishing email. |
| **C**ontainment | Stop it spreading and minimise the impact: isolate machines, disable compromised accounts | Isolate the host from the network so the attacker can't pivot to other systems |
| **E**radication | Remove the threat completely so the environment is clean | Run a deep malware scan to remove the malicious software |
| **R**ecovery | Restore systems from backup or rebuild them, test them, put them back in use | Reconfigure the host and restore exfiltrated data from backup |
| **L**essons Learned | Find and document the gaps in detection/response so next time goes better | Post-incident review meeting to find the root cause and improve security |

---

## Incident Response Plan

Organisations build their IR process from frameworks like SANS. The formal document listing all those procedures is called the **Incident Response Plan (IRP)**.

---

## Incident Response Tools

| Tool | What it does |
|---|---|
| **SIEM** | Collects all important logs in one central place and **correlates** them to spot incidents |
| **AV (Antivirus)** | Detects **known** malicious programs and regularly scans the system for them |
| **EDR** | Deployed on every endpoint, protects against more advanced threats, and can **contain and eradicate** them |

> 💡 AV = known bad stuff (signatures). EDR = behaviour + response. That's why EDR catches things AV misses.

---

## Playbooks vs Runbooks

| | Playbook | Runbook |
|---|---|---|
| **What** | High-level guidelines for handling a type of incident | Detailed, step-by-step execution of specific steps |
| **Level** | *What* to do | *How* exactly to do it |
| **Varies by** | Incident type | Resources/tools available |

### 📘 Example Playbook: Phishing Email

1. Notify all stakeholders of the phishing incident
2. Analyse the email header and body to decide if it's malicious
3. Look for attachments and analyse them
4. Check if anyone opened the attachments
5. Isolate infected systems from the network
6. Block the sender

---

# Part 4: Logs

## Why Logs Matter

| Use Case | Description |
|---|---|
| **Security events monitoring** | With real-time monitoring, logs help detect anomalous behaviour |
| **Incident investigation & forensics** | Logs are traces of every activity, so they show what happened during an incident. Used for root cause analysis. |
| **Troubleshooting** | Logs record system/app errors, which helps diagnose and fix issues |
| **Performance monitoring** | Insight into how applications are performing |
| **Auditing & compliance** | Logs create a trail of activity, which makes audits and compliance way easier |

---

## Log Types

| Log Type | Used For | Example Events |
|---|---|---|
| **System logs** | Troubleshooting OS issues | Startup/shutdown, driver loading, system errors, hardware events |
| **Security logs** | Detecting and investigating incidents | Authentication, authorisation, security policy changes, user account changes, abnormal activity |
| **Application logs** | Events inside a specific app | User interaction, app changes, app updates, app errors |
| **Audit logs** | System changes and user events; compliance + security monitoring | Data access, system changes, user activity, policy enforcement |
| **Network logs** | Incoming/outgoing traffic; network troubleshooting + investigations | Incoming/outgoing traffic, connection logs, firewall logs |
| **Access logs** | Who accessed which resources | Web server, database, application and API access logs |

---

## Important Windows Event IDs

| Event ID | Description |
|---|---|
| `4624` | A user account successfully logged in |
| `4625` | A user account failed to log in |
| `4634` | A user account successfully logged off |
| `4720` | A user account was created |
| `4722` | A user account was enabled |
| `4724` | An attempt was made to reset an account's password |
| `4725` | A user account was disabled |
| `4726` | A user account was deleted |

> 🔗 **Home lab link:** loads of `4625`s in a row from the same source is basically the Windows version of the failed SSH logins I see in `auth.log`. Brute force pattern.

---

## Reading a Web Server Access Log

Example entry:

```
172.16.0.1 - - [06/Jun/2024:13:58:44] "GET / HTTP/1.1" 200 "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
```

| Field | Value | Meaning |
|---|---|---|
| **IP address** | `172.16.0.1` | Who made the request |
| **Timestamp** | `[06/Jun/2024:13:58:44]` | When the request was made |
| **HTTP method** | `GET` | What action the server should perform |
| **URL** | `/` | The resource being requested |
| **Status code** | `200` | Server's response (200 = OK; different numbers = different results) |
| **User-Agent** | `Mozilla/5.0 (Macintosh…) Chrome/58…` | Info about the user's OS, browser, etc. |

---

# 🔑 Key Takeaways

- A SOC is all about **detection + response**, with L1 → L2 → L3 as an escalation ladder
- **SIEM** detects (logs), **EDR** detects + responds (endpoint), **firewall** blocks (network)
- Forensics follows NIST: **Collection → Examination → Analysis → Reporting**
- No authorisation = evidence might be useless in court
- **Chain of custody** + **write blockers** keep evidence trustworthy
- **Memory image before disk image**, because RAM is volatile
- Incident response follows **PICERL**: Preparation, Identification, Containment, Eradication, Recovery, Lessons Learned
- Not every alert is real: know your **true vs false positives**
- **Playbook** = what to do, **runbook** = exactly how
- Logs are the evidence trail for everything. Know your log types and key Windows Event IDs (`4624`, `4625` especially)
