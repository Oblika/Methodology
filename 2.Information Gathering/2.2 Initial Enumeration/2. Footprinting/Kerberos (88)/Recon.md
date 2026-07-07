### Service Detection with Nmap

```bash
nmap -p 88 -sV target.com
```

### Banner Grabbing
Use `netcat` to identify Kerberos servers and gather realm information:
```bash
# Using netcat (limited)
nc -vn target.com 88
```