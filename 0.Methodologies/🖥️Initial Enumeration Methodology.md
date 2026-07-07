
## 🟢 0. Objective

Pas fitimit të aksesit në një host (Linux/Windows/server), qëllimi është:

- Të kuptohet ambienti lokal dhe rrjeti
- Të identifikohen shërbime dhe privilegje të reja
- Të zbulohen rrugë për lateral movement dhe privilege escalation

---

# 🔵 1. Initial Intelligence Gathering (Local Context)

### Q1: A ke informacion për ambientin (domain, subnet, role të hostit)?

- YES →  
    ➜ Document:
    - Host role (server, workstation, DC)
    - Domain info
    - Network segment  
        ➜ Move to Host Discovery
- NO →  
    ➜ Extract:
    - hostname
    - OS version
    - network config  
        ➜ Proceed normally

---

# 🟡 2. Network Discovery (Internal Surface Mapping)

### Q2: A është hosti pjesë e një network më të madh?

- YES →  
    ➜ Perform internal discovery:
    
    - ARP scanning (local subnet)
    - ICMP sweep
    - TCP/UDP host discovery
    - Identify live hosts
    
    ➜ Document all discovered hosts
    
- NO →  
    ➜ Focus on single-host enumeration

---

# 🟣 3. Port & Service Discovery

### Q3: A ke mapped open ports?

- YES →  
    ➜ For each port:
    - Identify service
    - Identify version
    - Confirm with banner grabbing
    - Validate with scripts
- NO →  
    ➜ Run full TCP/UDP scan

---

# 🔴 4. Service Enumeration Layer

### Q4: Çfarë lloj shërbimi ke gjetur?

Për çdo shërbim:

- File sharing → SMB / FTP / NFS
- Remote access → SSH / RDP / WinRM
- Web services → HTTP/S
- Directory services → LDAP / Kerberos
- Database services → MySQL / MSSQL / PostgreSQL

👉 Për çdo kategori:

➜ “Run service-specific enumeration module”

---

# 🟠 5. Vulnerability Mapping

### Q5: A ekziston version i shërbimit?

- YES →  
    ➜ Match:
    - Exploit-DB
    - Metasploit modules
    - Known CVEs  
        ➜ Check misconfigurations (anonymous login, default creds)
- NO →  
    ➜ Focus on manual enumeration

---

### Q6: A ka misconfiguration?

- YES →  
    ➜ Exploit misconfiguration (priority path)
    - anonymous access
    - weak permissions
    - exposed shares
    - open admin panels
- NO →  
    ➜ Deep enumeration required

---

# 🔵 6. File Share & Credential Discovery

### Q7: A ka file sharing services?

- YES →  
    ➜ Test:
    - anonymous login
    - readable shares
    - writable shares
    - credential leakage
- NO →  
    ➜ Skip

---

# 🟣 7. Web Presence Check

### Q8: A ekziston web server në host/network?

- YES →  
    ➜ Switch to:  
    👉 Web Enumeration Methodology
- NO →  
    ➜ Continue internal service focus

---

# 🔴 8. Pivoting Decision Point

### Q9: A ke kredenciale / access reusable?

- YES →  
    ➜ Try lateral movement:
    - SSH reuse
    - SMB auth
    - WinRM
    - domain login
- NO →  
    ➜ Focus:
    - privilege escalation
    - credential harvesting

---

# 🧠 9. Escalation Path Selection

### Q10: Çfarë ke aktualisht?

- Low privilege user →  
    ➜ Local privilege escalation research
- Service account →  
    ➜ Token/credential extraction + lateral movement
- Admin/root →  
    ➜ Lateral movement + domain pivoting