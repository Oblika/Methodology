## 🟢 0. Objective

- Escalate from user → root
- OR extract credentials → lateral movement

---

# 🔵 1. QUICK WIN CHECK (FIRST PRIORITY)

### Q1: A ka direct misconfig exploit?

Check first:

- sudo -l misconfig
- SUID binaries
- writable cron jobs
- writable services
- capabilities abuse

👉 IF YES:  
➡ exploit immediately (NO further enumeration)

---

# 🟡 2. USER CONTEXT ANALYSIS

### Q2: Çfarë access kemi?

- normal user
- service user
- container user
- root-limited shell

Check:

- id
- groups
- sudo privileges

---

# 🟠 3. CREDENTIAL DISCOVERY (HIGH VALUE)

### Q3: A ka credentials?

Check:

- config files (/etc, /opt, /var/www)
- bash history
- ssh keys
- env variables
- backups

👉 IF FOUND:  
➡ try privilege escalation OR lateral access

---

# 🔴 4. PERMISSION-BASED ESCALATION

### Q4: A ka privilege abuse vectors?

Check:

- SUID / SGID
- sudo misconfig
- file permissions
- writable system files

Tools:

- GTFOBins logic

---

# 🟣 5. SERVICE & SCHEDULER ABUSE

### Q5: A ka scheduled execution?

Check:

- cron jobs
- systemd timers
- scripts running as root

👉 IF writable:  
➡ hijack execution path → root

---

# 🔵 6. CAPABILITIES & CONTAINERS

### Q6: A ka special privileges?

Check:

- Linux capabilities
- Docker group
- Kubernetes access
- privileged containers

---

# 🟤 7. NETWORK & INTERNAL ENUMERATION

Check:

- netstat services
- internal ports
- hidden services
- local-only applications

---

# ⚫ 8. PROCESS & SESSION ABUSE

Check:

- tmux sessions
- running processes
- tcpdump sniffing opportunities

---

# 🔴 9. APPLICATION & VERSION EXPLOITS

### Q7: A ka vulnerable software?

Check:

- installed packages
- kernel version
- service versions

👉 THEN:

- CVE search
- exploit-db
- GitHub PoCs
  
| CVE                           | Versioni i Prekur  | Komanda Verifikimi |
| ----------------------------- | ------------------ | ------------------ |
| Baron Samedit (CVE-2021-3156) | sudo < 1.9.5p2     | `sudo --version`   |
| Dirty Pipe (CVE-2022-0847)    | Kernel 5.8–5.16.11 | `uname -r`         |
| Polkit (CVE-2021-4034)        | pkexec <0.120      | pkexec --version   |

---

# 🧠 10. KERNEL / DEEP EXPLOITS (LAST RESORT)

Only IF:

- no misconfig
- no creds
- no weak perms

Then:

- kernel exploits
- sudo CVEs
- polkit / dirty pipe / netfilter