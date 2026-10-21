# Defensive Security & SOC

## Core SOC Responsibilities

### Detection
| What to Detect | Description |
|---------------|-------------|
| **Vulnerabilities** | Weaknesses in software/OS that attackers can exploit |
| **Unauthorized activity** | e.g. attacker using stolen credentials to log in |
| **Policy violations** | e.g. downloading pirated software, sending files insecurely |
| **Intrusions** | Unauthorized access to systems or networks |

### Response
- Minimise impact of detected incidents
- Perform root cause analysis
- Support the incident response team

---

## SOC Team Structure

| Role | Responsibilities |
|------|-----------------|
| **SOC Analyst L1** | First responder — basic alert triage, determines if detection is harmful, reports through proper channels |
| **SOC Analyst L2** | Deeper investigation — correlates data from multiple sources |
| **SOC Analyst L3** | Experienced — proactively hunts threats, leads containment, eradication and recovery |
| **Security Engineer** | Deploys and configures security solutions |
| **Detection Engineer** | Writes detection rules/logic behind security tools |
| **SOC Manager** | Manages SOC processes, reports to CISO on security posture |

---

## Incident Report Example

| Field | Detail |
|-------|--------|
| **What** | Malicious file detected on a host |
| **When** | 13:20, June 5 2024 |
| **Where** | Directory on "GEORGE PC" |
| **Who** | User: George |
| **Why** | Downloaded from a pirated software website to use software for free |

---

## Key SOC Tools

### SIEM (Security Information and Event Management)
- Collects logs from multiple network devices (log sources)
- Provides **detection** capabilities
- Used in almost every SOC environment

### EDR (Endpoint Detection and Response)
- Real-time and historical visibility of device activity
- Operates at the **endpoint** level
- Can carry out **automated responses**
- Allows detailed investigation and response with a few clicks

### Firewall
- Acts as a barrier between internal and external networks
- Monitors and filters incoming/outgoing traffic
- Blocks suspicious traffic before it reaches the internal network
- Operates at the **network** level

---

## Tool Comparison

| Tool | Level | Main Function | Can Respond? |
|------|-------|--------------|-------------|
| SIEM | Network | Log collection + detection | ❌ Detection only |
| EDR | Endpoint | Real-time visibility + detection | ✅ Automated response |
| Firewall | Network | Traffic filtering + blocking | ✅ Blocks traffic |

# Digital Forensics Fundamentals
Forensics is the application of methods and procedures to investigate and solve crimes. The branch of forensics that investigates cyber crimes is known as digital forensics. Cyber crime is any criminal activity conducted on or using a digital device.
he NIST works on defining frameworks for different areas of technology, including cyber security, where they introduce the process of digital forensics in four phases.

Collection: The first phase of digital forensics is data collection. Identifying all the devices from which the data can be collected is essential. Usually, an investigator can find personal computers, laptops, digital cameras, USBs, etc., on the crime scene. It is also necessary to ensure the original data is not tampered with while collecting the evidence and to maintain a proper document containing the collected items’ details. We will also be discussing the evidence-acquisition procedures in the upcoming tasks.
Examination: The collected data may overwhelm investigators due to its size. This data usually needs to be filtered, and the data of interest needs to be extracted. For example, as an investigator, you collected all the media files from a digital camera on the crime scene. You may only require some of the media as you are concerned with the media recorded on a specific date and time. So, in the examination phase, you would filter the media files of the required time and move them to the next phase. Similarly, you may only need the data of a specific user from a system containing numerous user accounts. The examination phase helps you filter this particular data for analysis.
Analysis: This is a critical phase. The investigators now have to analyze the data by correlating it with multiple pieces of evidence to draw conclusions. The analysis depends upon the case scenario and available data. The analysis aims to extract the activities relevant to the case chronologically.
Reporting: In the last phase of digital forensics, a detailed report is prepared. This report contains the investigation’s methodology and detailed findings from the collected evidence. It may also contain recommendations. This report is presented to law enforcement and executive management. It is important to include executive summaries as part of the report, considering the level of understanding of all the receiving parties.
As part of the collection phase, we saw that various pieces of evidence can be found at the crime scene. Analyzing these multiple categories of evidence requires various tools and techniques. 
Computer forensics: The most common type of digital forensics is computer forensics, which concerns investigating computers, the devices most commonly used in crimes.
Mobile forensics: Mobile forensics involves investigating mobile devices and extracting evidence such as call records, text messages, GPS locations, and more.
Network forensics: This area of forensics covers investigation beyond individual devices. It includes the whole network. The majority of the evidence found in networks is the network traffic logs.
Database forensics: Many critical data is stored in dedicated databases. Database forensics investigates any intrusion into these databases that results in data modification or exfiltration.
Cloud forensics: Cloud forensics is the type of forensics that involves investigating data stored on cloud infrastructure. This type of forensics sometimes gets tricky for the investigators as there is little evidence on cloud infrastructures.
The forensics team should obtain authorization from the relevant authorities before collecting any data. Evidence collected without prior approval may be deemed inadmissible in court. Forensic evidence contains private and sensitive data of an organization or individual. Proper authorization before collecting this data is essential for investigating according to the limits of the law.
Chain of Custody
Imagine that a team of investigators collects all the evidence from the crime scene, and some of the evidence goes missing after a few days, or there is any change in the evidence. No individual can be held accountable in this scenario because there is no proper process for documenting the evidence owners. This problem can be solved by maintaining a chain of custody document
Description of the evidence (name, type).
Name of individuals who collected the evidence.
Date and time of evidence collection.
Storage location of each piece of evidence.
Access times and the individual record who accessed the evidence.
se of Write Blockers
Write blockers are an essential part of the digital forensics team’s toolbox. Suppose you are collecting evidence from a suspect’s hard drive and attaching the hard drive to the forensic workstation. While the collection occurs, some background tasks in the forensic workstation may alter the timestamps of the files on the hard drive. This can cause hindrances during the analysis, ultimately producing incorrect results. Suppose the data was collected from the hard drive using a write blocker instead in the same scenario. This time, the suspect’s hard drive would remain in its original state as the write blocker can block any evidence alteration actions.

These forensic images are bit-by-bit copies of the whole operating system. Two different categories of forensic images are taken from a Windows operating system.

Disk image: The disk image contains all the data present on the storage device of the system (HDD, SSD, etc.). This data is non-volatile, meaning that the disk data would survive even after a restart of the operating system. For example, all the files like media, documents, internet browsing history, and more.

Memory image: The memory image contains the data inside the operating system’s RAM. This memory is volatile, meaning the data will get lost after the system is powered off or restarted. For example, to capture open files, running processes, current network connections, etc., the memory image should be prioritized and taken first from the suspect’s operating system; otherwise, any restart or shutdown of the system would result in all the volatile data getting deleted. While carrying out digital forensics on a Windows operating system, disk and memory images are very important to collect.
Email forensics: Email, the most common communication method between professionals, has become an important part of digital forensics. Emails are investigated to determine whether they are part of phishing or fraudulent campaigns.
We can try to read the metadata using the program pdfinfo. Pdfinfo displays various metadata related to a PDF file, such as title, subject, author, creator, and creation date.
ExifTool is used to read and write metadata in various file types, such as JPEG images
