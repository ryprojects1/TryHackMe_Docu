# 🛡️ Defensive Security, SOC & Digital Forensics

> TryHackMe notes. Covers what a SOC actually does, who does what, the main tools, and then the basics of digital forensics.

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

⚠️ The forensics team must get **authorisation from the relevant authorities before collecting any data**.

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

## ✅ Key Takeaways

- A SOC = **detect + respond**. L1/L2/L3 is an escalation ladder.
- **SIEM** detects across the network, **EDR** detects *and* responds on endpoints, a **firewall** blocks at the network edge.
- Forensics follows **Collection ➜ Examination ➜ Analysis ➜ Reporting**.
- Never touch evidence without **authorisation**, and always keep a **chain of custody**.
- Use **write blockers** so you don't accidentally change the evidence.
- Grab the **memory image before the disk image**, because RAM disappears when the power goes.
