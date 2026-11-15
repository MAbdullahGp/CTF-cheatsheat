# Linux Enumeration & Privilege Escalation

## 1. Finding SUID Binaries (Privesc ka pehla step)
Use case: Agar koi binary root ke permissions se chalta hai, to usay abuse karke privesc ho sakta hai.

```bash
find / -perm -4000 -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
```

Jo binary mile, usko **GTFOBins** (Linux) ya **LOLBAS** (Windows) par search karo:
> https://gtfobins.github.io/

```bash
sudo -l          # pehle check karo current user kya bina password sudo chala sakta hai
```

---

## 2. General Enumeration Commands

| Command | Purpose |
|---|---|
| `find / -type f -iname "*flag*" -exec cat {} + 2>/dev/null` | Flag files dhoondo |
| `grep -rnw '/etc/' -e 'flag{' 2>/dev/null` | Flag pattern search |
| `grep -rnw '/etc/' -e 'THM{' 2>/dev/null` | TryHackMe flag pattern |
| `find / -writable -type f 2>/dev/null` | Writable files dhoondo (replace karke priv-esc) |
| `ps aux` | Saare running processes (poori system) |
| `top` | Live resource usage monitor |
| `cat /etc/crontab` | Scheduled (automated) jobs dekho |
| `ls -la /etc/cron.d/` | Cron job files list |

**ps vs ps aux:** `ps` sirf current terminal window ke processes dikhata hai, `ps aux` **poori system** ke saare processes.

---

## 3. Important Paths to Always Check
```
/etc/ssh/sshd_config
~/.ssh/custom_key
~/.ssh/config
/usr/share/wordlists/rockyou.txt
/usr/share/seclists/
/usr/share/nmap/scripts
```

---

## 4. Getting an Interactive Shell
```bash
/usr/bin/python2.7 -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
Use case: Jab limited/restricted shell mile (e.g. rbash), full TTY shell banane ke liye.

---

## 5. Privilege Escalation Automation Tools

| Tool | OS | Purpose |
|---|---|---|
| **LinPEAS** | Linux | Privesc vectors automatically scan karta hai |
| **WinPEAS** | Windows | Same, Windows ke liye |
| **GTFOBins** | Linux | Misconfigured binaries se privesc |
| **LOLBAS** | Windows | Same concept, Windows binaries |

```bash
./linpeas.sh
```
Use case: Shell milne ke baad, manually har cheez check karne se pehle linpeas chala do — ye possible privesc paths highlight karega. Findings ko manually verify zaroor karo.

---

## 6. String & File Analysis

```bash
strings <filename> | grep -iE "flag|ctf|pass|key"
strings -n 3 Tetrix.exe | less      # -n 3 = minimum 3 char strings
```

```bash
exiftool <image.jpg>        # metadata nikalna
binwalk -e <image.jpg>      # embedded/hidden files extract karna
gpg --show-keys --with-fingerprint key.asc
```

---

## 7. Bastion/Jump Host Access Pattern
```bash
ssh -J bastion.example.com mark@internal-server
```
Use case: Jab internal server direct internet se accessible na ho, pehle ek approved gateway (bastion/jump host) se connect karte hain.
