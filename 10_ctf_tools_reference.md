# CTF Tools — Quick Reference Guide

**How to use:** Pehle identify karo tumhare paas kya hai (IP, website, service, file, ya shell), phir matching tool choose karo.

## 1. Reconnaissance & Web Discovery

| Tool | What | When |
|---|---|---|
| **Nmap** | Open ports/services scan | CTF ek IP de to yahan se shuru karo |
| **Gobuster** | Hidden folders/files | Web server milne ke baad |
| **FFUF** | Fast fuzzer (FUZZ keyword) | Directories, params, vhosts |
| **WhatWeb** | Site technology detect | Web enum ke shuru mein |
| **Nikto** | Common web issues check | Quick server check (leads only, proof nahi) |

Tip: Port 80/443 = web, 21 = FTP, 22 = SSH, 445 = SMB

## 2. Inspecting Web Requests
| Tool | Use |
|---|---|
| **cURL** | Headers, status codes, redirects, GET/POST |
| **Burp Suite** | Login forms, cookies, params, access control testing |

## 3. Network Services
| Tool | Use |
|---|---|
| **Dig/DNSRecon/DNSenum** | DNS records, subdomains |
| **SMBClient/Enum4linux-ng** | Port 445 open → shares/info check |
| **FTP client** | Port 21 open → anonymous access check |

## 4. Linux Shell & Password Investigation
| Tool | Use |
|---|---|
| **LinPEAS** | Authorized shell milne ke baad privesc paths |
| **GTFOBins** | `sudo -l`/SUID mein unusual binary mile to check karo |
| **John the Ripper** | Hash cracking against wordlist |
| **Hash Identifier** | Pehle hash type guess karo |

---

## 5. Quick CTF Decision Table

| Situation | Start with |
|---|---|
| IP address mila | Nmap → open ports/versions note karo |
| Web port open | WhatWeb → Gobuster/FFUF → Burp/cURL |
| Port 445 open | SMBClient / Enum4linux-ng |
| Port 21 open | FTP client, anonymous check |
| Domain mila | Dig / DNSRecon / DNSenum |
| Linux shell mila | `id` → `sudo -l` → SUID/capabilities → LinPEAS |
| Hash mila | Hash Identifier → John the Ripper |

**Golden Rule:** Enumeration pehle. Har tool blindly mat chalao — output parho, evidence follow karo, aur notes rakho (ports, paths, usernames, files, errors).

**Safety:** Yeh sab sirf authorized CTF boxes/labs par use karo.

**References:**
- HackTricks — https://book.hacktricks.wiki/
- GTFOBins — https://gtfobins.github.io/
- PortSwigger Academy — https://portswigger.net/web-security
