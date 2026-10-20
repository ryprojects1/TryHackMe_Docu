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
