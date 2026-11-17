# SOC Level 1 Notes

My notes from the TryHackMe SOC Level 1 path, covering alert triage, SOC metrics and EDR. Written in my own words while working through the rooms.

## Contents

- [Alert Triage](#alert-triage)
  - [What is an Alert?](#what-is-an-alert)
  - [SOC Roles](#soc-roles)
  - [Alert Verdicts](#alert-verdicts)
  - [Picking the Right Alert](#picking-the-right-alert)
  - [Triage Workflow](#triage-workflow)
  - [Alert Reporting](#alert-reporting)
- [SOC Metrics](#soc-metrics)
  - [Core Metrics](#core-metrics)
  - [Time-Based Metrics and SLAs](#time-based-metrics-and-slas)
  - [Fixing Bad Metrics](#fixing-bad-metrics)
- [EDR (Endpoint Detection and Response)](#edr-endpoint-detection-and-response)
  - [Why EDR?](#why-edr)
  - [EDR vs Antivirus](#edr-vs-antivirus)
  - [How EDR Works](#how-edr-works)
  - [Telemetry](#telemetry)
  - [Detection Techniques](#detection-techniques)
  - [Response Actions](#response-actions)
  - [EDR in the Bigger Picture](#edr-in-the-bigger-picture)

---

## Alert Triage

### What is an Alert?

An alert is a notification a security tool raises when something looks off, which an analyst then checks by hand. Companies generate millions of events a day, so nobody can read them all. Alerts are how the important ones get surfaced.

| Solution | Examples | Notes |
|---|---|---|
| SIEM | Splunk ES, Elastic | Good alert management, the default choice for most SOC teams |
| EDR / NDR | Microsoft Defender, CrowdStrike | Have their own dashboards, but alerts are usually routed into the SIEM or SOAR |
| SOAR | Splunk SOAR, Cortex SOAR | Used by bigger teams to pull alerts together in one place and automate parts of triage |
| ITSM | Jira, TheHive | Some teams use a separate ticketing system for alerts |

### SOC Roles

| Role | Responsibility |
|---|---|
| L1 Analyst | Triages alerts, separates real threats from noise, escalates real threats to L2 |
| L2 Analyst | Takes escalated alerts, digs deeper, handles remediation |
| SOC Engineer | Makes sure alerts carry enough info to triage them quickly |
| SOC Manager | Tracks how fast and how well alerts are being triaged |

### Alert Verdicts

|  | Alert fired | No alert |
|---|---|---|
| **Real threat** | 🔴 **True Positive (TP)**: real attack, caught | ⚠️ **False Negative (FN)**: real attack, missed |
| **No threat** | 🟢 **False Positive (FP)**: noise, harmless | ✅ **True Negative (TN)**: harmless, correctly ignored |

> [!IMPORTANT]
> False negatives are the dangerous ones. A missed attack, or a real alert wrongly closed as an FP, is what the Threat Detection Rate punishes.

### Picking the Right Alert

1. **Filter**: only pick up new, unassigned, unresolved alerts. Never grab one a teammate is already working.
2. **Sort by severity**: Critical > High > Medium > Low. Higher severity alerts are more likely to be real and high impact.
3. **Sort by time**: oldest first. If there are two active breaches, the older one has had longer to do damage.

### Triage Workflow

**Initial actions**

1. Assign the alert to yourself
2. Set it to *In Progress*
3. Read the alert name, description and key indicators

**Investigation** (if there's no playbook to follow)

- **Who** is affected: user, hostname, cloud account, network, website
- **What** happened: suspicious login, malware, phishing, etc.
- **Where** it happened: device, IP, URL
- **When** it happened: check events just before and after the alert
- **Why** you think it's malicious or not: verify with threat intel platforms

**Final actions**

1. Decide on a verdict: TP or FP
2. Write a detailed comment covering what you checked and why you reached that verdict
3. **TP, or not sure** → escalate to L2
4. **FP** → close the alert

> [!TIP]
> Escalating when you're unsure is fine. Closing a real attack as an FP is much worse than an extra escalation.

### Alert Reporting

| Why write reports? | Why it matters |
|---|---|
| Context for escalation | L2 can pick it up without starting from scratch |
| Record keeping | SIEM logs are only kept for 3 to 12 months, but alerts and their comments are kept indefinitely |
| Skill development | Writing it up forces you to actually understand what you found |

**Report format: the 5 Ws**

| W | What to include |
|---|---|
| Who | User, account or entity involved |
| What | The exact action or sequence of events |
| When | Start and end time of the suspicious activity |
| Where | Device, IP address or website involved |
| Why | Your reasoning for the verdict |

---

## SOC Metrics

### Core Metrics

| Metric | Formula | What it shows | Healthy target |
|---|---|---|---|
| Alerts Count (AC) | Total alerts received | Workload on analysts | ~5 to 30 alerts per L1 per day |
| False Positive Rate (FPR) | FPs / Total alerts | How noisy the alerts are | Below 80% (0% isn't realistic) |
| Alert Escalation Rate (AER) | Escalated alerts / Total alerts | How experienced and independent L1s are | Below 50%, ideally below 20% |
| Threat Detection Rate (TDR) | Detected threats / Total threats | How reliable the SOC is | **Always 100%** |

**Notes on each**

- **Alerts Count**: too many (e.g. 80 in the queue at the start of a shift) and real threats get buried. Too few is also a warning sign, since it can mean the SIEM is broken or the SOC has blind spots.
- **FPR**: if 75 out of 80 alerts are FPs (94%), analysts start treating every alert as spam and stop paying attention. 80% or higher means detection rules need tuning, which is called *False Positive Remediation*.
- **AER**: L2 relies on L1 to filter out noise. L1s also shouldn't close alerts they don't fully understand, so this is a balance.
- **TDR**: example: 6 attacks in a year, 4 caught, 1 missed because of a broken detection rule, 1 closed as an FP by mistake. TDR = 4/6 = 67%, which is very bad. Every miss is a potential ransomware or data exfiltration incident.

### Time-Based Metrics and SLAs

| Metric | Common SLA | Meaning |
|---|---|---|
| SOC Availability | 24/7 | Team schedule, usually 8/5 (Mon to Fri) or 24/7 |
| Mean Time to Detect (MTTD) | 5 min | Average time from the attack to the tools detecting it |
| Mean Time to Acknowledge (MTTA) | 10 min | Average time for L1 to start triaging a new alert |
| Mean Time to Respond (MTTR) | 60 min | Average time from the alert arriving to the breach being stopped (includes MTTA) |

```
Attack ──MTTD──▶ Alert ──MTTA──▶ Triage starts ────▶ Breach stopped
                   └──────────────── MTTR ─────────────────┘
```

### Fixing Bad Metrics

| Problem | Threshold | How to fix it |
|---|---|---|
| High FPR | > 80% | Exclude trusted activity (e.g. system updates) from EDR/SIEM rules. Automate triage of common alerts with SOAR or scripts |
| Slow MTTD | > 30 min | Get SOC engineers to make detection rules run faster or more often. Check SIEM logs arrive in real time, not 10 minutes late |
| Slow MTTA | > 30 min | Make sure analysts get notified of new alerts in real time. Spread alerts evenly across the analysts on shift |
| Slow MTTR | > 4 hours | As L1, escalate real threats to L2 as quickly as possible. Make sure the team has documented playbooks for different attack scenarios |

---

## EDR (Endpoint Detection and Response)

### Why EDR?

EDR has three main strengths:

- **Visibility**: it records detailed activity on each endpoint, including processes, registry changes, file and folder changes and user actions. You get the full process tree, a timeline of events and historical data for threat hunting.
- **Detection**: it uses both signature-based and behaviour-based detection, plus ML to spot deviations from normal behaviour. It can catch fileless malware running in memory and accepts custom IOCs.
- **Response**: you can act straight from the console, e.g. isolate a host, kill a process, quarantine a file or open a remote shell.

### EDR vs Antivirus

**Airport analogy**

- **Antivirus = passport control.** It checks each person against a list of known criminals. If someone isn't on the list, they get in, even a professional thief with a clean record.
- **EDR = security staff inside the terminal.** They watch CCTV and motion sensors and notice behaviour: someone hanging around restricted areas, acting suspiciously, leaving bags unattended.

**Example attack chain: malicious Word doc**

| Step | Attack | AV response | EDR response |
|---|---|---|---|
| 1 | File downloaded | Nothing, if the hash isn't in its signature database | Logs the download and keeps watching |
| 2 | Document opened | Nothing, since `winword.exe` is legitimate | Records `winword.exe` execution |
| 3 | Macro runs | Nothing, if the macro has no known signature | Flags `winword.exe` spawning `powershell.exe` as an unusual parent-child relationship |
| 4 | Obfuscated PowerShell runs | Usually misses obfuscated scripts | Flags the obfuscated script |
| 5 | Code injected into `svchost.exe` | Doesn't monitor memory injection | Detects the process injection |
| 6 | `svchost.exe` connects out | No network visibility | Flags the unexpected outbound connection |
| **Result** | | May mark the machine as clean ✅ | Raises an alert with the **full attack chain** and lets the analyst respond 🚨 |

### How EDR Works

- **Agents (sensors)**: installed on each endpoint, they're the eyes and ears. They monitor activity, stream it to the console in real time and can run basic detections themselves.
- **Console**: the brain. It correlates data from every agent, matches it against threat intel and uses logic and ML to join the dots into a detection (an alert).

**After a detection:**

1. The EDR assigns a severity: Critical, High, Medium, Low or Informational
2. The analyst acknowledges the alert and picks the highest severity first
3. The analyst reviews the details: files, processes, network connections, registry changes
4. The analyst decides whether it's a TP or FP
5. If it's a TP, the analyst takes action from the console

### Telemetry

Telemetry is the data agents send to the console. It's like a plane's black box for the endpoint.

| Telemetry type | What it helps catch |
|---|---|
| Process executions and terminations | Suspicious parent-child relationships, dodgy executables, malware payloads |
| Network connections | C2 traffic, unusual ports, data exfiltration, lateral movement |
| Command line activity | Malicious CMD/PowerShell commands and obfuscated scripts AV usually misses |
| File and folder modifications | Data staging, ransomware encrypting files, dropped malicious files |
| Registry modifications | Persistence and config changes (the registry holds a lot of Windows config) |

### Detection Techniques

| Technique | How it works | Example |
|---|---|---|
| Behavioural | Looks at what a file *does*, not just its signature | `winword.exe` spawning `powershell.exe` |
| Anomaly | Learns each endpoint's normal baseline and flags deviations (can cause FPs, but the context helps check them) | A process changing an auto-start registry key on a machine where that never normally happens |
| IOC matching | Matches activity against threat intel feeds (works for everything except zero-days) | A dropped executable's hash matches a known attack tool |
| MITRE ATT&CK mapping | Tags each detection with the attack stage (tactic and technique) | Scheduled task creation → **Tactic:** Persistence, **Technique:** Scheduled Task/Job |
| Machine learning | Models trained on normal and malicious behaviour spot chains of individually harmless actions | Fileless attacks, multi-stage intrusions |

### Response Actions

EDR supports **automated** responses (policies that block known bad behaviour) and **manual** responses:

| Action | What it does | When to use it |
|---|---|---|
| Isolate host | Cuts the endpoint off from the network | Stop lateral movement, since most attacks start on one machine and spread |
| Terminate process | Kills a specific process | When isolating would hurt the business more than the threat (e.g. critical servers). Be careful, since killing a legit process can break things |
| Quarantine | Moves a file somewhere it can't run | Malicious files. Review later to restore or delete |
| Remote access | Opens a shell on the endpoint (e.g. CrowdStrike Falcon RTR) | When the built-in actions aren't enough. Run scripts or dig deeper |
| Artefact collection | Pulls forensic data remotely | Forensics or legal reporting: memory dumps, event logs, folder contents, registry hives |

### EDR in the Bigger Picture

EDR on its own gives a lot of endpoint coverage, but it sits alongside other tools: firewalls, DLP, email security gateways, IAM and so on. To keep things efficient, they all feed into a **SIEM**, which becomes the main place analysts investigate from.

```
Firewall ─┐
DLP ──────┤
Email GW ─┼──▶ SIEM ──▶ SOC analyst
IAM ──────┤
EDR ──────┘
```
