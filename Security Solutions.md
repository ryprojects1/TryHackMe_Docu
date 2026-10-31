# Malware Analysis Tools

## Table of Contents

- [CAPA: The Basics](#capa-the-basics)
- [REMnux](#remnux)
- [FlareVM](#flarevm)

---

# CAPA: The Basics

CAPA (Common Analysis Platform for Artifacts) is a tool by FireEye Mandiant that identifies capabilities in executable files (PE, ELF, .NET, shellcode, sandbox reports) by applying rules that describe common malware behaviours.

## CLI Options

| Option | Description | Example |
|--------|-------------|---------|
| `-h` / `--help` | Show help message | `capa -h` |
| `-v` / `--verbose` | Enable verbose output | `capa.exe .\cryptbot.bin -v` |
| `-vv` / `--vverbose` | Enable very verbose output | `capa.exe .\cryptbot.bin -vv` |

## First Block Output Fields

| Field | What it shows |
|-------|---------------|
| md5 / sha1 / sha256 | Cryptographic hashes of the file |
| analysis | How CAPA performed its analysis |
| os | OS context for identified capabilities |
| arch | Architecture (e.g. x86) |
| path | Location of the analysed file |

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

## Micro-Objectives

| Micro-Objective | Description |
|-----------------|-------------|
| PROCESS | Creating processes, setting thread context, checking mutex |
| MEMORY | Allocating, changing protection, or freeing memory |
| COMMUNICATION | DNS, FTP, HTTP, ICMP, SMTP network traffic |
| DATA | Checking strings, compressing, encoding, decoding data |

## Behaviour Examples

| Objective | Behaviour | ID | Explanation |
|-----------|-----------|----|-------------|
| Anti-Behavioral Analysis | Lab Machine Detection | B0009 | Checks if running in a virtual environment |
| Anti-Static Analysis | Executable Code Obfuscation | B0032 | Intentionally obscures code to prevent static analysis |
| Execution | Command and Scripting Interpreter | E1059 | Exploits cmd.exe, PowerShell, Bash, Python, Perl, JavaScript |
| Discovery | File and Directory Discovery | E1083 | Searches for specific files by enumerating directories |
| Anti-Static Analysis / Defense Evasion | Obfuscated Files or Information | E1027 | Encodes or encrypts files/samples to hinder analysis |

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

## Namespace Examples

| Namespace | Rule File | What it detects |
|-----------|-----------|-----------------|
| `anti-analysis/anti-vm/vm-detection` | `reference-anti-vm-strings-targeting-virtualbox.yml` | Malware searching for VMware registry keys or VM tools |
| `anti-analysis/obfuscation` | `obfuscated-with-dotfuscator.yml` | String encryption, code obfuscation, packing, anti-debugging |

---

# REMnux

## Oledump.py

`oledump.py` is a Python tool that analyzes OLE2 files, commonly called Structured Storage or Compound File Binary Format. OLE stands for Object Linking and Embedding, a proprietary technology developed by Microsoft. OLE2 files are typically used to store multiple data types, such as documents, spreadsheets, and presentations, within a single file. This tool is handy for extracting and examining the contents of OLE2 files, making it a valuable resource for forensic analysis and malware detection.

Based on OleDump's file analysis, a VBA script might be embedded in the document and found inside `xl/vbaProject.bin`. Therefore, oledump will assign this with an index of `A`, though this can sometimes differ. The A (index) + numbers are called **data streams**.

Let's run the command:

```bash
oledump.py agenttesla.xlsm -s 4
```

This command will run oledump and look into the actual data stream of interest using the parameter `-s 4`, wherein the `-s` parameter is short for `--select` and the number four (4) as the data stream of interest is in the 4th place (`A4: M 688 'VBA/ThisWorkbook'`).

### Let's break it down!

| Parameter | Meaning |
|-----------|---------|
| `-WindowStyle hidden` | Controls how the PowerShell window appears when executing a script or command. `hidden` means the PowerShell window won't be visible to the user. |
| `-executionpolicy bypass` | By default, PowerShell restricts script execution for security reasons. This parameter overrides that policy. `bypass` means the execution policy is temporarily ignored, allowing any script to run without restriction. |
| `Invoke-WebRequest` | Commonly used for downloading files from the internet. |
| `-Uri` | Specifies the URL of the web resource you want to retrieve. In our case, the script is downloading `Doc-3737122pdf.exe` from `http://193.203.203.67/rt/`. |
| `-OutFile` | Specifies the local file where the downloaded content will be saved. In this case, `Doc-3737122pdf.exe` will be saved to `$TempFile`. |
| `Start-Process` | Used to execute the downloaded file stored in `$TempFile` after the web request. |

### Summary

When the document `agenttesla.xlsm` is opened, a **Macro** will run! This Macro contains a VBA script. The script runs PowerShell to download a file named `Doc-3737122pdf.exe` from `http://193.203.203.67/rt/`, save it to a variable `$TempFile`, then execute the file inside this variable, which is a binary or `.exe` file (`Doc-3737122pdf.exe`). This is a usual technique used by threat actors to avoid early detection. Pretty nasty, right?!

## Volatility

Here are some of the parameters or plugins we will use. We will focus on Windows plugins.

- `windows.pstree.PsTree`
- `windows.pslist.PsList`
- `windows.cmdline.CmdLine`
- `windows.filescan.FileScan`
- `windows.dlllist.DllList`
- `windows.malfind.Malfind`
- `windows.psscan.PsScan`

### Let's break this command down, shall we?

- We created a variable named `$plugin` with values of each Volatility plugin
- Then ran `vol3` with parameters `-q`, which means quiet mode (does not show the progress in the terminal)
- And `-f`, which means read from the memory capture
- `$plugin > wcry.$plugin.txt` means run Volatility with the plugins and output it to a file with `wcry` at the beginning, followed by the name of the plugin, with a `.txt` extension. Repeat until every value of `$plugin` is used.

---

# FlareVM

FlareVM, or "Forensics, Logic Analysis, and Reverse Engineering," stands out as a comprehensive and carefully curated collection of specialized tools uniquely designed to meet the specific needs of reverse engineers, malware analysts, incident responders, forensic investigators, and penetration testers. This toolkit, expertly crafted by the FLARE Team at FireEye, is a powerful aid in unravelling digital mysteries, gaining insight into malware behaviour, and delving into the complex details within executables.

## Reverse Engineering & Debugging

Reverse engineering is like solving a puzzle backward: you take a finished product apart to understand how it works. Debugging is identifying errors, understanding why they happen, and correcting the code to prevent them.

| Tool | Description |
|------|-------------|
| Ghidra | NSA-developed open-source reverse engineering suite |
| x64dbg | Open-source debugger for binaries in x64 and x32 formats |
| OllyDbg | Debugger for reverse engineering at the assembly level |
| Radare2 | A sophisticated open-source platform for reverse engineering |
| Binary Ninja | A tool for disassembling and decompiling binaries |
| PEiD | Packer, cryptor, and compiler detection tool |

## Disassemblers & Decompilers

Disassemblers and Decompilers are crucial tools in malware analysis. They help analysts understand malicious software's behaviour, logic, and control flow by breaking it into a more understandable format.

| Tool | Description |
|------|-------------|
| CFF Explorer | A PE editor designed to analyze and edit Portable Executable (PE) files |
| Hopper Disassembler | A debugger, disassembler, and decompiler |
| RetDec | Open-source decompiler for machine code |

## Static & Dynamic Analysis

Static and dynamic analysis are two crucial methods in cyber security for examining malware. Static analysis involves inspecting the code without executing it, while dynamic analysis involves observing its behaviour as it runs.

| Tool | Description |
|------|-------------|
| Process Hacker | Sophisticated memory editor and process watcher |
| PEview | A portable executable (PE) file viewer for analysis |
| Dependency Walker | A tool for displaying an executable's DLL dependencies |
| DIE (Detect It Easy) | A packer, compiler, and cryptor detection tool |

## Forensics & Incident Response

Digital Forensics involves the collection, analysis, and preservation of digital evidence from various sources like computers, networks, and storage devices. At the same time, Incident Response focuses on the detection, containment, eradication, and recovery from cyberattacks.

| Tool | Description |
|------|-------------|
| Volatility | RAM dump analysis framework for memory forensics |
| Rekall | Framework for memory forensics in incident response |
| FTK Imager | Disc image acquisition and analysis tools for forensic use |

## Network Analysis

Network Analysis includes different methods and techniques for studying and analysing networks to uncover patterns, optimize performance, and understand the underlying structure and behaviour of the network.

| Tool | Description |
|------|-------------|
| Wireshark | Network protocol analyzer for traffic recording and examination |
| Nmap | A vulnerability detection and network mapping tool |
| Netcat | Read and write data across network connections with this helpful tool |

## File Analysis

File Analysis is a technique used to examine files for potential security threats and ensure proper file permissions.

| Tool | Description |
|------|-------------|
| FileInsight | A program for looking through and editing binary files |
| Hex Fiend | Hex editor that is light and quick |
| HxD | Binary file viewing and editing with a hex editor |

## Scripting & Automation

Scripting and Automation involve using scripts such as PowerShell and Python to automate repetitive tasks and processes, making them more efficient and less prone to human error.

| Tool | Description |
|------|-------------|
| Python | Mainly automation-focused on Python modules and tools |
| PowerShell Empire | Framework for PowerShell post-exploitation |

## Sysinternals Suite

The Sysinternals Suite is a collection of advanced system utilities designed to help IT professionals and developers manage, troubleshoot, and diagnose Windows systems.

| Tool | Description |
|------|-------------|
| Autoruns | Shows what executables are configured to run during system boot-up |
| Process Explorer | Provides information about running processes |
| Process Monitor | Monitors and logs real-time process/thread activity |

## Investigative Value

| Tool | Investigative Value |
|------|---------------------|
| Procmon | A helpful tool for tracking system activity, especially regarding malware research, troubleshooting, and forensic investigations |
| Process Explorer | Allows you to see the parent-child relationship of processes, DLLs loaded, and their path |
| HxD | Malicious files can be examined or altered via hex editing |
| Wireshark | Observing and investigating network traffic to look for unusual activity |
| CFF Explorer | Can generate file hashes for integrity verification, authenticate the source of system files, and validate their validity |
| PEStudio | Static analysis or studying executable file properties without running the files |
| FLOSS | Extracts and de-obfuscates all strings from malware programs using advanced static analysis techniques |

## FLOSS

Using advanced static analysis techniques, the FLARE Obfuscated String Solver (FLOSS, formerly FireEye Labs Obfuscated String Solver) automatically extracts and de-obfuscates all strings from malware programs. Like `strings.exe`, it can enhance the basic static analysis of unknown binaries. FLOSS also includes more Python scripts in the scripts directory, which can be used to load the script's output into other programs like IDA Pro or Binary Ninja.

## PEStudio: Imported Functions

The functions tab lists API calls that the file has imported. This is also known as the **IAT (Import Address Table)**. By clicking on the blacklist tab, PEStudio will sort the API by moving all the blacklisted functions to the top. This is useful because it enables us to understand how malware may behave once it compromises a host.

Here are the important functions that we noted:

| Function(s) | What it indicates |
|-------------|-------------------|
| `set_UseShellExecute` | Allows the process to use the operating system's shell to execute other processes. This is often seen in malware that spawns additional processes to carry out malicious actions. |
| `CryptoStream`, `RijndaelManaged`, `CipherMode`, `CreateDecryptor` | The executable uses cryptographic functions, specifically Rijndael (AES encryption). Malware may use cryptography to encrypt communication and files or even implement ransomware functionality. |
