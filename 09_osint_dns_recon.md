# OSINT & DNS Reconnaissance

## 1. DNS Lookup Tools

```bash
dig example.com MX              # mail servers
dig +short txt ch whoami.cloudflare @1.1.1.1   # apni public info Cloudflare resolver se
dnsrecon -d example.com
dnsenum
nslookup
whois
```
Use case: Jab CTF domain name de, to pehle DNS records (A, MX, TXT, NS) aur domain ownership details nikalo.

### DNS Zone Transfer
```bash
dig axfr @nameserver domain.com
```
Use case: Misconfigured DNS server se **poori zone file** (sab subdomains/records) ek saath nikal lena — agar zone transfer allowed ho to bohot bara info leak hota hai.

---

## 2. OSINT Websites & Services

| Service | Purpose |
|---|---|
| whoisxmlapi.com | WHOIS data API |
| domaintools.com | Domain history/ownership |
| securitytrails.com | DNS history, subdomains |
| whoxy.com | WHOIS lookup |
| viewdns.info | Multiple DNS/IP tools |
| Google Admin Toolbox | DNS/network diagnostics |
| Have I Been Pwned | Leaked credentials check |
| harvester (theHarvester) | Domain se emails gather karna |
| Shodan / Censys | Internet-exposed devices/services search engine |

---

## 3. Process & System Monitoring
```bash
ps aux    # poori system ke processes
ps        # sirf current terminal
top       # live resource monitor
```

---

## 4. DNS/Network Security Terms (Glossary)

| Term | Meaning |
|---|---|
| **DNS tunnelling** | DNS queries ke andar data chupa kar exfiltrate/communicate karna |
| **DGA (Domain Generation Algorithm)** | Malware randomly domains generate karta hai C2 ke liye |
| **Fast Flux** | Ek domain ko bar bar different IPs se resolve karwana (detection avoid karne ke liye) |
| **Open Resolver** | DNS server jo kisi bhi client ke queries resolve kar de — DDoS amplification mein abuse hota hai |
| **DNS Cache Poisoning** | Fake DNS response inject karke user ko galat IP par bhejna |
| **DNSSEC** | DNS responses ko cryptographically sign karna (authenticity verify) |
| **DANE** | TLS certificates ko DNS (DNSSEC) ke through verify karna |
| **SPF/DMARC** | Email spoofing rokne wale DNS-based mechanisms |
| **Geofencing** | Location ke basis par access restrict karna |
| **Cloudflare Anycast** | Same IP multiple locations se serve hona (CDN technique) |

---

## 5. Security Concepts (General Recon-Adjacent)

| Term | Meaning |
|---|---|
| **EDR (Endpoint Detection & Response)** | Endpoint par malicious activity detect/respond karne wala tool |
| **Lateral Movement** | Ek compromised machine se network ke andar aagay badhna |
| **DLP (Data Loss Prevention)** | Sensitive data leak hone se rokne wale controls |
| **Principle of Least Privilege** | User ko sirf utni hi permission do jitni zaroorat ho |
| **Risk Assessment Value** | Kisi threat ka impact x likelihood |
| **Behavioural Analysis** | Normal se hat kar activity detect karna (anomaly detection) |
