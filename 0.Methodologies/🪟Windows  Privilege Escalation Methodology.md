## 🟢 0. Objective

Qëllimi:

- From low privilege → SYSTEM / Admin
- Or credential access → lateral movement
- Run winPEASx64.exe

---

# 🔵 1. QUICK WIN CHECK (HIGHEST PRIORITY)

### Q1: A ka immediate privilege misconfig?

Check first:

- SeImpersonatePrivilege
- SeDebugPrivilege
- SeTakeOwnershipPrivilege
- Weak service permissions
- Scheduled tasks writable

👉 IF YES:  
➡ exploit immediately (DO NOT CONTINUE ENUMERATION)

---

# 🟡 2. USER CONTEXT ANALYSIS

### Q2: Çfarë user access kemi?

- Standard user
- Service account
- Local admin

Check:

- whoami /priv
- whoami /groups

👉 Output determines attack path

---

# 🟠 3. CREDENTIAL DISCOVERY (HIGH VALUE)

### Q3: A ka credentials të ekspozuara?

Check:

- cmdkey /list
- saved passwords
- browser creds
- config files
- shares

👉 IF credentials found:  
➡ test lateral movement first  
➡ privesc second

---

# 🔴 4. SERVICE & APPLICATION ABUSE

### Q4: A ka services vulnerable?

Check:

- service permissions
- unquoted paths
- weak binaries
- auto-start services

👉 IF writable service found:  
➡ service hijacking → SYSTEM

---

# 🟣 5. GROUP-BASED ESCALATION

### Q5: A jemi në privileged groups?

Check:

- Backup Operators
- Event Log Readers
- Print Operators
- Server Operators
- Hyper-V Admins
- DNSAdmins

👉 IF yes:  
➡ group-specific abuse path (FAST TRACK)

---

# 🔵 6. FILE & DATA ENUMERATION

### Q6: A ka sensitive files?

Check:

- config files
- scripts
- logs
- database creds
- unattended files

Tools:

- manual search
- Snaffler

---

# 🟤 7. INTERNAL SYSTEM ENUMERATION

Check:

- netstat -ano
- internal services
- localhost ports
- hidden admin panels

---

# ⚫ 8. PERSISTENCE / USER INTERACTION

Check:

- scheduled tasks
- LNK / SCF attacks
- user interaction vectors

---

# 🔴 9. ADVANCED ESCALATION PATHS

### Q7: A ka kernel / OS exploit?

Only IF:

- no misconfig found
- no creds found

Then:

- kernel exploits
- CVEs
- UAC bypass

---

# 🧠 10. FINAL DECISION LAYER

At this point:

- Either SYSTEM achieved
- Or credentials harvested for lateral movement