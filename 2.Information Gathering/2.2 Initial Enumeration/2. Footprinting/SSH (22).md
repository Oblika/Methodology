

```bash
# Connect to ssh service
ssh <user>@<IP>

# If you have the private key, you can log in to a remote server using SSH.
ssh -i path/to/private_key user@target-ip

# Enforce password-based authentication
ssh <user>@<FQDN/IP> -o PreferredAuthentications=password

# Use to fingerprint the SSH server
./ssh-audit.py <IP>

# User Enumeration with Metasploit
msf> use auxiliary/scanner/ssh/ssh_enumusers
```

**Përdorimi:**

- Hyrje në server përmes SSH
- Testim i password authentication
- SSH security auditing (ciphers, key exchange, CVEs)