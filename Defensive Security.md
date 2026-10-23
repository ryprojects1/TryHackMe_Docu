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

## Incident Response Fundamentals
False positive: A security solution raised an alert on a high amount of data being transferred from one system to an external IP address. Upon analyzing this alert, the security team found that the subject system was undergoing a backup process to a cloud storage service, which caused this. This is known as a false positive.

True Positive: A security solution raised an alert on a phishing attempt on one of the organization’s users. Upon analyzing this alert, the security team found that the email was a phishing email sent to this user to compromise the system. This is known as a true positive.

There are several other types of incidents. These types can occur independently or altogether within the same victim.
Malware Infections: Malware is a malicious program that can damage a system, network, or application. The majority of incidents are associated with malware infections.  There are different types of malware, each with a unique potential to cause damage.
Security Breaches: Security Breaches arise when an unauthorized person gets access to confidential data (something we don’t want them to see or have).  
Data Leaks: Data leaks are incidents in which confidential information of an individual or an organization is exposed to unauthorized entities. Many attackers use data leaks for reputational damage to their victims or use this technique to threaten their victims and get what they need from them. 
Insider Attacks: Incidents from within an organization are known as insider attacks. Think about a disgruntled employee infecting the whole network through a USB on his last day. 
Denial Of Service Attacks: Availability is one of the three pillars of cyber security. Defensive security solutions and people constantly find ways to protect information; they ensure that the data is available to the people simultaneously.
The SANS incident Response framework has 6 phases, which can be called 'PICERL' to remember them easily.

Phase	Explanation	Example
Preparation	This is the first phase. The preparation phase includes building the necessary resources to handle an incident. These resources include developing incident response teams, having a proper incident response plan in place, and deploying necessary security solutions to combat the incidents.	Conducting awareness training for employees on phishing emails. Phishing emails are fraudulent emails sent by malicious attackers that can trick you into performing actions that can lead you to an incident.
Identification	The identification phase refers to looking for any abnormal behavior that may indicate an incident. This involves using various security solutions and techniques to monitor abnormal events.	The security team notices a huge amount of data being sent out from one of the hosts. Upon analysis, it was found to be compromised after a malicious file was downloaded from a phishing email attachment.
Containment	Once an incident has been identified, the next step should be to contain it. This means minimizing the impact of the attack. This is usually done by isolating the victim machine, disabling the compromised user accounts, etc.	The Security team isolates the host from the network to minimize the impact and not allow the attacker to jump to other systems, leveraging the compromised host.
Eradication	This phase, as its name suggests, involves removing the threat from the attacked environment. The threat may be of any kind. The eradication phase will ensure the subject environment is clean, and now we can move to the recovery phase.	A deep malware scan was executed on the system to remove the malicious software from the host.
Recovery	The recovery phase is very important in this chain. It involves recovering the affected systems from backup or rebuilding them. The recovered systems are then tested and are ready to use.	The compromised host was re-configured, and the exfiltrated data was restored from the backup.
Lessons Learned	This is also an important part of the incident response lifecycle. Gaps in the detection and analysis of the incident are identified and documented, helping to improve the overall process in future incidents.	Conducting a post-incident review meeting to analyze the incident's root cause and improve the security to prevent future attacks.
Organizations may derive their incident response processes by following these frameworks. Every process has a formal document listing all the relevant organizational procedures. The formal incident response document is called the Incident Response Plan. 
SIEM: The Security Information and Event Management Solution (SIEM) collects all important logs in one centralized location and correlates them to identify incidents.
AV: Antivirus (AV) detects known malicious programs in a system and regularly scans your system for these.
EDR: Endpoint Detection and Response (EDR) is deployed on every system, protecting it against some advanced-level threats. This solution can also contain and eradicate the threat.
Playbooks are the guidelines for a comprehensive incident response.

Following is an example of a Playbook for an incident: Phishing Email

Picture showing a playbook.

Notify all the stakeholders of the phishing email incident
Determine if the email was malicious by conducting header and body analysis of the email
Look for any attachments with the email and analyze them
Determine if anybody opened the attachments
Isolate the infected systems from the network
Block the email sender
Runbooks, on the other hand, are the detailed, step-by-step execution of specific steps during different incidents. These steps may vary depending on the resources available for investigation.

Use Case	Description
Security Events Monitoring	Logs help us detect anomalous behavior when real-time monitoring is used.
Incident Investigation and Forensics	Logs are the traces of every kind of activity. It offers detailed information on what happened during the incident. The security team utilizes the logs to perform root cause analysis of incidents.
Troubleshooting	As the logs also record the errors in systems or applications, they can be used to diagnose issues and helpful in fixing them.
Performance Monitoring	Logs can also provide valuable insights into the performance of applications.
Auditing and Compliance	Logs play a major role in Auditing and Compliance, making it easier with its capability to establish a trail of different kinds of activities.

Log Type	Usage	Example
System Logs	The system logs can be helpful in troubleshooting running issues in the OS. These logs provide information on various operating system activities.	- System Startup and shutdown events
- Driver Loading events
- System Error events
- Hardware events
Security Logs	The security logs help detect and investigate incidents. These logs provide information on the security-related activities in the system.	-Authentication events
- Authorization events
- Security Policy changes events
- User Account changes events - Abnormal Activity events
Application Logs	The application logs contain specific events related to the application. Any interactive or non-interactive activity happening inside the application will be logged here.	- User Interaction events
- Application Changes events
- Application Update events
- Application Error events
Audit Logs	The Audit logs provide detailed information on the system changes and user events. These logs are helpful for compliance requirements and can play a vital role in security monitoring as well.	- Data Access events
- System Change events
- User Activity events
- Policy Enforcement events
Network Logs	Network logs provide information on the network’s outgoing and incoming traffic. They play crucial roles in troubleshooting network issues and can also be handy during incident investigations.	- Incoming Network Traffic events
- Outgoing Network Traffic events
- Network Connection Logs - Network Firewall Logs
Access Logs	The Access logs provide detailed information about the access to different resources. These resources can be of different types, providing us with information on their access.	- Webserver Access Logs
- Database Access Logs - Application Access Logs
- API Access Logs
Here is a table of some important Event IDs in Windows Operating System.

Event ID	Description
4624	A user account successfully logged in
4625	A user account failed to login
4634	A user account successfully logged off
4720	A user account was created
4724	An attempt was made to reset an account’s password
4722	A user account was enabled
4725	A user account was disabled
4726	A user account was deleted

IP Address: “172.16.0.1” - The IP address of the user who made the request.

Timestamp: “[06/Jun/2024:13:58:44]” - The time when the request was made to the website.

Request: The request details.

HTTP Method: “GET” - Tells the website what action to be performed on the request.
URL: “/” - The requested resource.
Status Code: “200” - The response from the server. Different numbers indicate different response results.

User-Agent: “Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36” - Information about the user’s Operating System, browser, etc. when making the request.
