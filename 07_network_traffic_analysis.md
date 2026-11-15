# Network Traffic Analysis

## 1. Tcpdump — Live Packet Capture

```bash
sudo tcpdump port 110 -A         # POP3 traffic (-A = ASCII output)
sudo tcpdump host 10.20.30.148 -A   # specific host ka traffic
sudo tcpdump port 80 -A          # HTTP traffic (POST requests mein creds mil sakti hain)
sudo tcpdump port 21 -A          # FTP (cleartext credentials)
sudo tcpdump -w capture.pcap     # file mein save karo baad mein analysis ke liye
tcpdump -r capture.pcap -A       # saved capture read karo
```
Use case: Cleartext protocols (HTTP, FTP, POP3, Telnet) ka traffic capture karke unme se credentials ya sensitive data nikalna.

---

## 2. Other Sniffing/Analysis Tools (Reference)

| Tool | Purpose |
|---|---|
| **Tshark** | Wireshark ka CLI version |
| **tcpflow** | TCP streams ko reconstruct karta hai |
| **ngrep** | Network traffic mein grep jaisi searching |
| **NetworkMiner** | Passive network forensics (files/creds extract) |
| **Bettercap / Ettercap** | MITM attacks (ARP spoofing etc.) |
| **mitmproxy** | HTTPS traffic intercept/modify karna |
| **CyberChef** | Encoding/decoding/analysis Swiss-army knife (web tool) |
| **Responder** | LLMNR/NBT-NS poisoning (Windows networks) |

---

## 3. Path Monitoring

```bash
mtr <target>          # real-time traceroute + packet loss stats
traceroute <target>   # path trace karo
```

---

## 4. Remote Connection Tools

```bash
telnet TARGET PORT      # cleartext remote access / banner grabbing
nc TARGET PORT          # netcat — generic TCP/UDP connections
ncat --ssl TARGET PORT  # netcat ka TLS-supported version
openssl s_client -connect TARGET:PORT   # TLS connection test/debug
stunnel                 # kisi bhi service ko TLS tunnel mein wrap karna
```
Use case: Banner grabbing, manual protocol testing, ya encrypted tunnel set karna jab service khud encryption support na kare.

---

## 5. TLS/SSL Testing

| Tool | Purpose |
|---|---|
| `testssl.sh` | TLS configuration/vulnerabilities check karna |
| `sslyze` | TLS scanning (Python-based) |
| SSL Labs (ssllabs.com) | Online TLS grading |
| `nmap --script ssl-enum-ciphers` | Supported ciphers list karna |

---

## 6. CDN/WAF Identification Note
Agar target IP in ranges mein aaye: `104.16.0.0/12`, `172.64.0.0/13`, `198.41.0.0/16` — ye **Cloudflare CDN/WAF** nodes hain, direct server IP nahi.

---

## 7. Quick File/Metadata Checks
```bash
binwalk <file>              # embedded files dhoondo
exiftool sakurapwnedletter.svg   # metadata extract (images/docs)
```
