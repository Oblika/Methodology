## 🟢 0. Objective

Qëllimi në AD environment është:

- Identifikimi i domain structure
- Zbulimi i users, groups, machines
- Gjetja e misconfigurations
- Credential harvesting
- Lateral movement
- Privilege escalation deri Domain Admin

---

# 🔵 1. Initial Network Discovery (Unauthenticated)

### Q1: A ke akses në network (internal foothold)?

- NO →  
    ➜ Need initial access (phishing, exploit, web foothold)
- YES →  
    ➜ Start AD enumeration

---

## 1.1 Host Discovery

### Q2: A mund të identifikosh domain / DC / hosts?

- YES →  
    ➜ Document:
    - Domain Controller(s)
    - File servers
    - Workstations
    - Naming patterns
- NO →  
    ➜ Perform:
- ARP discovery (same subnet)
- ICMP sweep
- TCP/UDP scan
- mDNS / LLMNR / NBNS sniffing

---

## 1.2 Network Intelligence

- Identify:
    - Domain name
    - IP range
    - DNS structure
    - AD environment presence
- Observe traffic:
    - LLMNR
    - NBNS
    - mDNS

---

# 🟡 2. User Discovery (Unauthenticated)

### Q3: A mund të enumerosh users pa credentials?

- YES →  
    ➜ Methods:
    - SMB null session
    - LDAP anonymous bind
    - RPC enumeration
    - Kerbrute username spraying
    - SID enumeration
- NO →  
    ➜ Move to credential acquisition phase

---

## 2.1 User Intelligence Goals

- Domain users list
- Service accounts
- Admin groups
- Naming patterns

---

## 2.2 Pre-auth Attack Surface

Check:

- AS-REP roastable users
- Kerberos pre-auth disabled accounts

---

# 🟠 3. Initial Foothold (Credential Capture)

### Q4: A ke captured credentials?

- YES →  
    ➜ Move to credentialed enumeration
- NO →  
    ➜ Attempt:
- LLMNR/NBT-NS poisoning
- Password spraying
- Default credentials testing

---

## Credential Harvesting Sources

- NTLM hashes (Responder)
- Cached credentials
- Misconfigured shares
- Config files

---

# 🔵 4. Credentialed Enumeration

### Q5: A ke valid domain credentials?

- YES →  
    ➜ Full AD enumeration unlocks
- NO →  
    ➜ Return to foothold phase

---

## 4.1 Domain Mapping

- Domain users
- Groups
- Computers
- Trust relationships

Tools:

- BloodHound
- LDAP queries
- ldapdomaindump

---

## 4.2 Access Mapping

Identify:

- Who can RDP
- Who can WinRM
- Who has admin rights
- Service account privileges

---

# 🟣 5. Attack Path Discovery (BloodHound Phase)

### Q6: A ka privilege escalation paths?

- YES →  
    ➜ Prioritize shortest path:
    - ACL abuse
    - Kerberoasting
    - DCSync
    - GPO abuse
- NO →  
    ➜ Expand enumeration scope

---

## Key Attack Types

- Kerberoasting
- AS-REP roasting
- ACL abuse (GenericAll, WriteDACL)
- Unconstrained delegation
- Delegation abuse

---

# ADCS

Kontrollo per Certifikatat qe mund te ken ndonje dobesi me `certipy`.



---

# 🔴 6. Privilege Escalation Paths

### Q7: A ke access në privileged accounts?

- YES →  
    ➜ Move laterally or escalate to Domain Admin
- NO →  
    ➜ Exploit:
- Kerberoasting (SPNs)
- Password reuse
- GPP / stored creds
- Weak ACLs
- Misconfigurations

---

## High Value Misconfigurations

- Passwords in description fields
- Unconstrained delegation
- GPO write access
- Admin shares exposed
- LDAP signing disabled

---

# 🟤 7. Lateral Movement

### Q8: A ke access në other machines?

- YES →  
    ➜ Pivot:
    - SMB
    - WinRM
    - RDP
    - PSExec
- NO →  
    ➜ Return to credential hunting

---

## Pivot Decision Logic

- Credentials valid → reuse across hosts
- Local admin → move laterally
- Service account → escalate privileges

---

# ⚫ 8. Advanced Attacks

- Kerberos abuse:
    - Golden Ticket
    - Silver Ticket
    - Pass-the-Hash
    - Pass-the-Ticket
- DCSync attack (if privileges allow)
- Trust exploitation:
    - Parent domain attack
    - Cross forest attack

---

# 🔵 9. Persistence & Control

- Create new domain users (if allowed)
- Scheduled tasks
- Golden ticket persistence
- Credential dumping

---

# 🟢 10. Validation / Cleanup

- Confirm:
    - Domain Admin achieved?
    - Lateral movement complete?
    - Full domain visibility?