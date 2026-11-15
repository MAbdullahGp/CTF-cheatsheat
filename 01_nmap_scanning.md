# Nmap — Scanning Cheat Sheet

## 1. Host Discovery (Ping Scans)
Use case: Pata karna ke network mein kaun se hosts zinda (live) hain, bina ports scan kiye.

| Scan Type | Command | Explanation |
|---|---|---|
| ARP Scan | `sudo nmap -PR -sn 10.200.6.0/24` | Local network ke liye sabse reliable, ARP request bhejta hai |
| ICMP Echo | `sudo nmap -PE -sn 10.200.6.0/24` | Normal ping (echo request) |
| ICMP Timestamp | `sudo nmap -PP -sn 10.200.6.0/24` | Timestamp request se host zinda hai ya nahi check karta hai |
| ICMP Address Mask | `sudo nmap -PM -sn 10.200.6.0/24` | Subnet mask request |
| TCP SYN Ping | `sudo nmap -PS22,80,443 -sn 10.200.6.0/30` | SYN packet specific ports par bhejta hai |
| TCP ACK Ping | `sudo nmap -PA22,80,443 -sn 10.200.6.0/30` | ACK packet bhejta hai, firewall bypass ke liye useful |
| UDP Ping | `sudo nmap -PU53,161,162 -sn 10.200.6.0/30` | UDP ports par ping |
| Simple ping | `ping -4 -c 5 MACHINE_IP` | 5 ICMP packets bhejta hai |

**Important flags:**
- `-sn` → sirf host discovery, port scan nahi karega
- `-n` → DNS lookup skip (fast)
- `-R` → reverse-DNS lookup sab hosts ke liye
- Agar `-sn` nahi lagaya to Nmap by default live hosts par port scan bhi kar dega

---

## 2. Port Scan Types

| Scan | Command | Explanation |
|---|---|---|
| TCP SYN (Stealth) | `-sS` | Full handshake complete nahi karta, fast aur stealthy |
| TCP Connect | `-sT` | Full 3-way handshake, non-root ke liye |
| TCP ACK | `-sA` | Sirf ACK bhejta hai — firewall rules map karne ke liye (open/closed detect nahi karta) |
| TCP Window | `-sW` | ACK jaisa hi, but RST packet ka window size check karta hai |
| Null Scan | `sudo nmap -sN TARGET` | Koi flag set nahi — closed ports se response milta hai |
| FIN Scan | `sudo nmap -sF TARGET` | Sirf FIN flag — firewall ke peechay se bhi response mil sakta hai |
| Xmas Scan | `sudo nmap -sX TARGET` | FIN+PSH+URG — stateless firewalls ko bypass karne ki koshish |
| Maimon Scan | `sudo nmap -sM TARGET` | FIN+ACK — kuch BSD systems ka bug exploit karta hai |
| Custom Scan | `sudo nmap --scanflags URGACKPSHRSTSYNFIN TARGET` | Apni marzi ke flags combine karo |

**Note:** Null, FIN, Xmas → sirf **closed** ports response dete hain (open ports silent rehte hain).
Maimon, ACK, Window → **open aur closed dono** se response milta hai, lekin "open" confirm nahi karte, sirf filtering map karte hain.

---

## 3. Full / Fast Workflow (Real CTF Approach)

```bash
# Step 1: Sab ports jaldi scan karo (lightweight)
nmap -p- --min-rate 5000 -T4 <target-ip>

# Step 2: Jo ports open milay unki detail nikalo
nmap -p <found-ports> -sCV <target-ip>

# Step 3: UDP ke common ports bhi check karo (backdoors/DNS/SNMP ke liye)
nmap -sU --top-ports 100 <target-ip>
```

---

## 4. Service, OS & Script Detection

| Flag | Meaning |
|---|---|
| `-sV` | Service/version detect |
| `-sV --version-light` | Fast (2 probes) |
| `-sV --version-all` | Thorough (9 probes) |
| `-O` | OS detection |
| `--traceroute` | Path to target |
| `-sC` / `--script=default` | Default safe scripts |
| `-A` | = `-sV -O -sC --traceroute` (all-in-one, noisy) |
| `--script=SCRIPTS` | Custom script(s), e.g. `./vulscan/vulscan.nse,vuln` |

### NSE Script Categories
| Category | Use |
|---|---|
| `auth` | Authentication check |
| `brute` | Brute-force login |
| `default` | Same as `-sC` |
| `discovery` | Info gathering (DB tables, DNS names) |
| `dos` | DoS vulnerability detect |
| `exploit` | Actual exploitation attempt |
| `vuln` | Vulnerability checking |
| `safe` | Target crash nahi karega |

---

## 5. Output Saving
| Flag | Format |
|---|---|
| `-oN` | Normal |
| `-oG` | Grepable |
| `-oX` | XML |
| `-oA` | Normal + XML + Grepable, sab ek saath |

---

## 6. Evasion & Spoofing Techniques

| Technique | Command | Explanation |
|---|---|---|
| Spoof source IP | `sudo nmap -S SPOOFED_IP -Pn -e eth0 TARGET` | Traffic kisi aur IP se aata dikhega |
| Spoof MAC | `--spoof-mac Apple` / `00:11:22:33:44:55` / `0` (random) | Sirf LAN ke andar kaam karta hai — internet par MAC strip ho jata hai |
| Decoy Scan | `nmap -D 10.10.0.1,10.10.0.2,ME TARGET_IP` | Real IP ko decoys ke beech chupata hai |
| Fragmented Packets | `-f` (8 bytes) / `-ff` (16 bytes) | Firewall/IDS evade karne ke liye packets todta hai |
| Idle/Zombie Scan | `sudo nmap -sI ZOMBIE_IP TARGET` | Kisi third idle host ko use karke apni IP chupana |
| Custom source port | `--source-port PORT_NUM` (or `-g 20`) | Firewall bypass — port 20/53 jaisi trusted port se traffic bhejna |
| Random/extra data | `--data-length NUM` | Packet size change karke signature-based detection avoid karna |

**Debugging:** `-v`/`-vv` verbose, `-d`/`-dd` debug details, `--reason` batata hai Nmap ne decision kaise liya.

---

## Quick Reference Table (Host Discovery options)
| Option | Purpose |
|---|---|
| `-n` | No DNS lookup |
| `-R` | Reverse DNS for all hosts |
| `-sn` | Host discovery only, no port scan |
