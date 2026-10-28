# CyberChef: The Basics

CyberChef is a web-based tool for performing cyber operations directly in your browser.

---

## Core Operations

| Operation | Description | Example |
|-----------|-------------|---------|
| **From Morse Code** | Translates Morse Code into uppercase alphanumeric characters | `- .... .-. . .- - ...` → `THREATS` |
| **URL Encode** | Encodes special characters into percent-encoding for URIs/URLs | `https://tryhackme.com` → `https%3A%2F%2Ftryhackme%2Ecom` |
| **To Base64** | Encodes raw data into ASCII Base64 string | `This is fun!` → `VGhpcyBpcyBmdW4h` |
| **To Hex** | Converts input to hexadecimal bytes separated by a delimiter | `This Hex conversion is awesome!` → `54 68 69 73 20 48 65 78...` |
| **To Decimal** | Converts input to ordinal integer array | `This Decimal conversion is awesome!` → `84 104 105 115 32 68 101...` |
| **ROT13** | Caesar substitution cipher, rotates alphabet characters by 13 (default) | `Digital Forensics and Incident Response` → `Qvtvgny Sberafvpf naq Vapvqrag Erfcbafr` |

---

## Extractors

| Operation | Description |
|-----------|-------------|
| **Extract IP addresses** | Extracts all IPv4 and IPv6 addresses from input |
| **Extract URLs** | Extracts URLs from input (protocol required e.g. HTTP, FTP) |
| **Extract email addresses** | Extracts all email addresses from input |
