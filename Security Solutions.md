# CAPA: The Basics

CAPA (Common Analysis Platform for Artifacts) is a tool by FireEye Mandiant that identifies capabilities in executable files (PE, ELF, .NET, shellcode, sandbox reports) by applying rules that describe common malware behaviours.

---

## CLI Options

| Option | Description | Example |
|--------|-------------|---------|
| `-h` / `--help` | Show help message | `capa -h` |
| `-v` / `--verbose` | Enable verbose output | `capa.exe .\cryptbot.bin -v` |
| `-vv` / `--vverbose` | Enable very verbose output | `capa.exe .\cryptbot.bin -vv` |

---

## First Block Output Fields

| Field | What it shows |
|-------|--------------|
| md5 / sha1 / sha256 | Cryptographic hashes of the file |
| analysis | How CAPA performed its analysis |
| os | OS context for identified capabilities |
| arch | Architecture (e.g. x86) |
| path | Location of the analysed file |

---

## MITRE ATT&CK Objectives

| Objective | Description |
|-----------|-------------|
| Anti-Behavioral Analysis | Avoids detection by hindering sandboxes or debuggers |
| Anti-Static Analysis | Obstructs static analysis to hide malicious intent |
| Collection | Gathers information from the target machine or network |
| Command and Control | Establishes communication with C2 servers to control compromised systems |
| Credential Access | Steals account credentials such as usernames and passwords |
| Defense Evasion | Bypasses detection and security mechanisms |
| Discovery | Collects info about system/network configuration |
| Execution | Runs unauthorised commands or code without user consent |
| Exfiltration | Steals and extracts sensitive data from systems |
| Impact | Manipulates, disrupts, or damages systems and data |
| Lateral Movement | Spreads through the network actively or passively |
| Persistence | Remains undetected and operational for extended periods |
| Privilege Escalation | Gains elevated permissions or control within a system |

---

## Micro-Objectives

| Micro-Objective | Description |
|-----------------|-------------|
| PROCESS | Creating processes, setting thread context, checking mutex |
| MEMORY | Allocating, changing protection, or freeing memory |
| COMMUNICATION | DNS, FTP, HTTP, ICMP, SMTP network traffic |
| DATA | Checking strings, compressing, encoding, decoding data |

---

## Behaviour Examples

| Objective | Behaviour | ID | Explanation |
|-----------|-----------|-----|-------------|
| Anti-Behavioral Analysis | Lab Machine Detection | B0009 | Checks if running in a virtual environment |
| Anti-Static Analysis | Executable Code Obfuscation | B0032 | Intentionally obscures code to prevent static analysis |
| Execution | Command and Scripting Interpreter | E1059 | Exploits cmd.exe, PowerShell, Bash, Python, Perl, JavaScript |
| Discovery | File and Directory Discovery | E1083 | Searches for specific files by enumerating directories |
| Anti-Static Analysis / Defense Evasion | Obfuscated Files or Information | E1027 | Encodes or encrypts files/samples to hinder analysis |

---

## CAPA Namespaces

CAPA groups rules by purpose using namespaces.

| Namespace | Purpose |
|-----------|---------|
| `anti-analysis` | Detects obfuscation, packing, anti-debugging behaviours |
| `collection` | Data-gathering behaviours for exfiltration |
| `communication` | Network interactions including C2 communications |
| `compiler` | Identifies build environments or compilers used |
| `data-manipulation` | Data transformation - string encryption, data encoding |
| `executable` | Attributes in executable files (PE sections, debug info) |
| `host-interaction` | Reading, writing, or modifying files and directories |
| `impact` | Potential harm - remote access, exfiltration, destruction |
| `internal` | Internal rules not for direct analyst use |
| `lib` | Building blocks for creating other rules |
| `linking` | Detecting dynamically loaded external libraries |
| `load-code` | Runtime code injection behaviours |
| `malware-family` | Signatures linked to known malware families |
| `nursery` | Staging ground for unpolished rules |
| `persistence` | Maintaining access within a compromised system |
| `runtime` | Identifies language or platform the program runs on |
| `targeting` | Behaviours related to ATM interactions |

---

## Namespace Examples

| Namespace | Rule File | What it detects |
|-----------|-----------|----------------|
| `anti-analysis/anti-vm/vm-detection` | `reference-anti-vm-strings-targeting-virtualbox.yml` | Malware searching for VMware registry keys or VM tools |
| `anti-analysis/obfuscation` | `obfuscated-with-dotfuscator.yml` | String encryption, code obfuscation, packing, anti-debugging |
