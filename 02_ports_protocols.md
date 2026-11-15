# Ports & Protocols — Reference

## Memory Trick (Pattern)
- **TCP:** "FTP → SSH → Telnet → Mail → Web" — interactive/reliable services
- **UDP:** "DNS + DHCP + TFTP + NTP + SNMP" — fast/simple, connectionless

---

## TCP Ports

| Port | Protocol | Use Case | Security |
|---|---|---|---|
| 21 | FTP | File Transfer | Cleartext |
| 990 | FTPS | File Transfer | Encrypted (implicit TLS) |
| 22 | SSH / SFTP | Remote access, encrypted file transfer | Encrypted |
| 23 | Telnet | Remote access (obsolete) | Cleartext |
| 25 | SMTP | Server-to-server mail (MTA↔MTA) | Cleartext (STARTTLS optional) |
| 587 | SMTP Submission | Client sending mail (MUA→MSA), auth required | STARTTLS |
| 465 | SMTPS | Mail submission | Encrypted (implicit TLS) |
| 80 | HTTP | Web | Cleartext |
| 443 | HTTPS | Web | Encrypted (implicit TLS) |
| 110 | POP3 | Email retrieval | Cleartext |
| 995 | POP3S | Email retrieval | Encrypted |
| 143 | IMAP | Email sync | Cleartext (STARTTLS upgrade possible) |
| 993 | IMAPS | Email sync | Encrypted |
| 3306 | MySQL/MariaDB | Database | — |
| 5432 | PostgreSQL | Database | — |
| 3389 | RDP | Windows GUI remote access | — |

### Dev/Framework default ports (apni notes se)
- MERN → 3000
- Next.js → 3001
- Django → 8000
- Apache → 8080

---

## UDP Ports

| Port | Protocol | Use Case |
|---|---|---|
| 53 | DNS | Name resolution |
| 67/68 | DHCP (Server/Client) | Auto IP assignment |
| 69 | TFTP | Simple file transfer |
| 123 | NTP | Time sync |
| 161 | SNMP | Network device monitoring |

---

## Protocol Deep Dives

### POP3 (Port 110 / 995)
Upgrade to TLS via `STLS` command.

| Command | Description |
|---|---|
| `USER username` | Identify user |
| `PASS password` | Authenticate |
| `STAT` | Message count + size |
| `LIST` | List messages |
| `RETR n` | Retrieve message n |
| `DELE n` | Mark message n for deletion |
| `RSET` | Undo deletions |
| `QUIT` | End session, apply deletions |

### IMAP (Port 143 / 993)
Upgrade to TLS via `STARTTLS`. Har command se pehle ek unique tag (c1, c2...) lagta hai.

| Command | Description |
|---|---|
| `LOGIN username password` | Authenticate |
| `LIST "" "*"` | List mailbox folders |
| `SELECT folder` | Open folder (read/write) |
| `EXAMINE folder` | Open folder (read-only) |
| `FETCH n BODY[]` | Get message n |
| `SEARCH criteria` | Search messages |
| `STORE n +FLAGS (\Seen)` | Mark as read |
| `LOGOUT` | End session |

### SMTP
- Port 25 → server-to-server, often ISP-blocked for home connections
- Port 587 → recommended for sending mail via client, needs auth
- Port 465 → implicit TLS from the start

### SFTP vs FTPS vs SCP
- **SFTP** — runs over SSH (port 22), fully encrypted, most common FTP replacement
- **FTPS** — FTP + TLS, implicit on port 990, or STARTTLS on port 21
- **SCP** — also over SSH, being deprecated in favor of SFTP

### DNS Encryption Variants
| Name | Port | Note |
|---|---|---|
| DNS over TLS (DoT) | 853 | Encrypted DNS |
| DNS over HTTPS (DoH) | 443 | Encrypted DNS, blends with HTTPS traffic |
| DoH/DoT public resolver example | — | 1.1.1.1 |

---

## TCP Header Flags (Important for scanning + traffic analysis)

| Flag | Meaning |
|---|---|
| **URG** | Urgent pointer field significant — process immediately |
| **ACK** | Acknowledgement number significant |
| **PSH** | Push data to application promptly |
| **RST** | Reset the connection |
| **SYN** | Initiate 3-way handshake, sync sequence numbers |
| **FIN** | Sender has no more data to send |

---

## Quick Use-Case Table
- Web port open → go to Web Exploitation notes
- Port 445 open → SMB enumeration
- Port 21 open → check anonymous FTP
- Domain given → DNS/OSINT recon
- Hash given → Password Attacks notes
