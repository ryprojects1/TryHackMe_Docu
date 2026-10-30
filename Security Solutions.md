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

# REMnux
Oledump.py is a Python tool that analyzes OLE2 files, commonly called Structured Storage or Compound File Binary Format. OLE stands for Object Linking and Embedding, a proprietary technology developed by Microsoft. OLE2 files are typically used to store multiple data types, such as documents, spreadsheets, and presentations, within a single file. This tool is handy for extracting and examining the contents of OLE2 files, making it a valuable resource for forensic analysis and malware detection.
Based on OleDump's file analysis, a VBA script might be embedded in the document and found inside xl/vbaProject.bin. Therefore, oledump will assign this with an index of A, though this can sometimes differ. The A (index) +Numbers are called data streams. 
Let's run the command oledump.py agenttesla.xlsm -s 4. This command will run the oledump and look into the actual data stream of interest using the parameter -s 4,  wherein the -s parameter is short for -select  and the number four(4) as the data stream of interest is in the 4th place(A4: M 688 'VBA/ThisWorkbook')
Let's break it down!

So, in PowerShell, running the -WindowStyle parameter allows you to control how the PowerShell window appears when executing a script or command. In this case, hidden means that the PowerShell window won’t be visible to the user.
By default, PowerShell restricts script execution for security reasons. The -executionpolicy parameter allows you to override this policy. The bypass means that the execution policy is temporarily ignored, allowing any script to run without restriction.
The Invoke-WebRequest is commonly used for downloading files from the internet.
The -Uri Specifies the URL of the web resource you want to retrieve. In our case, the script is downloading the resource Doc-3737122pdf.exe from http://193.203.203.67/rt/.
The -OutFile specifies the local file where the downloaded content will be saved.  In this case, the Doc-3737122pdf.exe will be saved to $TempFile.
The Start-Process is used to execute the downloaded file that is stored in $TempFile after the web request.
To summarize, when the document agenttesla.xlsm is opened, a Macro will run! This Macro contains a VBA script. The script will run and will be running a PowerShell to download a file named Doc-3737122pdf.exe from http://193.203.203.67/rt/, save it to a variable $TempFile, then execute or start running the file inside this variable, which is a binary or a .exe file (Doc-3737122pdf.exe). This is a usual technique used by threat actors to avoid early detection. Pretty nasty, right?!
Here are some of the parameters or plugins we will use. We will focus on Windows plugins.

windows.pstree.PsTree
windows.pslist.PsList
windows.cmdline.CmdLine
windows.filescan.FileScan
windows.dlllist.DllList
windows.malfind.Malfind
windows.psscan.PsScan

Let’s break this command down, shall we?

We created a variable named $plugin with values of each volatility plugin
Then ran vol3 parameters -q, which means quiet mode or does not show the progress in the terminal
And -f, which means read from the memory capture.
The plugin > wcry.plugin.done; means run volatility with the plugins and output it to a file with wcry at the beginning of the text, followed by the name of the plugins and with an extension of .txt. Repeat until the value of variable $plugin is used.
