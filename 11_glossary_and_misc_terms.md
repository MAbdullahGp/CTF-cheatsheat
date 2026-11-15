# Glossary & Misc Security Terms

## TLS/SSL Related Attacks
| Term | Meaning |
|---|---|
| **SSL Stripping** | HTTPS connection ko silently HTTP mein downgrade karna (MITM) |
| **Fake Certificates** | Jaali TLS certificate use karke user ko trick karna |
| **Compromised/Rogue CAs** | Hacked ya fake Certificate Authority jo galat certs issue kare |
| **HSTS (HTTP Strict Transport Security)** | Browser ko force karta hai sirf HTTPS use kare, HTTP downgrade na ho |

## DNS Security Terms
| Term | Meaning |
|---|---|
| **DNS Tunnelling** | DNS queries mein data chupa kar exfiltrate karna |
| **DGA (Domain Generation Algorithms)** | Malware ke liye random domains generate karna |
| **Fast Flux** | Domain ko rapidly multiple IPs se resolve karna (detection evade) |
| **Open Resolver** | Har kisi ke DNS queries resolve karne wala server (abuse-prone) |
| **DNS Cache Poisoning** | Fake response inject karke DNS cache corrupt karna |
| **DNSSEC** | DNS ko cryptographic signing se secure karna |
| **DANE** | DNSSEC ke through TLS cert verify karna |
| **DoT / DoH** | Encrypted DNS (port 853 / 443) |

## General Security Concepts
| Term | Meaning |
|---|---|
| **EDR** | Endpoint Detection and Response |
| **Lateral Movement** | Compromise ke baad network mein aagay badhna |
| **DLP** | Data Loss Prevention |
| **Principle of Least Privilege** | Minimum necessary permissions dena |
| **Risk Assessment Value** | Threat impact × likelihood |
| **SPF/DMARC** | Email spoofing protection (DNS-based) |
| **Geofencing** | Location-based access restriction |
| **Cloudflare Anycast** | Same IP, multiple global locations se serve |
| **Behavioural Analysis** | Anomaly / abnormal activity detection |
| **Bastion/Jump Host** | Gateway server jo internal network tak controlled access deta hai |

---

## TCP Flags (bhi 02_ports_protocols.md mein hai, quick recall ke liye yahan bhi)
| Flag | Meaning |
|---|---|
| URG | Urgent data, immediately process karo |
| ACK | Acknowledgement number valid hai |
| PSH | Data turant application ko do |
| RST | Connection reset |
| SYN | Connection initiate / sequence sync |
| FIN | Sender ka data khatam |

---

## Advanced Nmap Port Scan Types (summary)
| Scan | Description |
|---|---|
| Null Scan | No flags set — closed ports se response milta hai |
| FIN Scan | Sirf FIN flag |
| Xmas Scan | FIN+PSH+URG — stateless firewall bypass attempt |
| Maimon Scan | FIN+ACK — BSD systems ka bug exploit |
| ACK Scan | Firewall rules map karta hai (open/closed detect nahi) |
| Window Scan | RST ke TCP Window field se open/closed differentiate |
| Custom Scan | `--scanflags` se apni flags banao |

## Evasion Techniques Summary
| Technique | Flag |
|---|---|
| Spoof IP | `-S` |
| Spoof MAC | `--spoof-mac` |
| Decoy Scan | `-D` |
| Fragmented Packets | `-f` / `-ff` |
| Idle/Zombie Scan | `-sI` |

---

## Misc Tools Master List (Quick Lookup)

**Recon/OSINT:** whois, dig, nslookup, dnsenum, dnsrecon, theHarvester, Shodan, Censys, viewdns.info

**Web:** Gobuster, FFUF, WhatWeb, Nikto, Burp Suite, dirb, feroxbuster

**Network Sniffing:** Tcpdump, Tshark, tcpflow, ngrep, NetworkMiner, Bettercap, Ettercap, mitmproxy, Responder

**Password Attacks:** Hydra, Medusa, Ncrack, CrackMapExec/NetExec, John the Ripper, Hashcat, CrackStation, Have I Been Pwned

**TLS Testing:** testssl.sh, sslyze, SSL Labs, nmap ssl-enum-ciphers

**Privesc:** LinPEAS, WinPEAS, GTFOBins, LOLBAS

**Misc:** CyberChef, binwalk, exiftool, strings, hash-identifier, mtr, traceroute, telnet, netcat/ncat, stunnel
