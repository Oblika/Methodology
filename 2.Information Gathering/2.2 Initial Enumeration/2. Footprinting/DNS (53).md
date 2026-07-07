
```bash
# Local DNS Configuration
cat /etc/bind/named.conf.local

#SOA (Start of Authority) Record
dig soa www.inlanefreight.com

# Zone Files (local)
cat /etc/bind/db.domain.com

# Reverse Name Resolution Zone Files
cat /etc/bind/db.10.129.14
 
# Requests information about the name servers managed by DNS
dig ns <domain.ltd> @<IP>

# Versioni i DNS serverit (CHAOS class query)
dig CH TXT version.bind 10.129.120.85

# I will request any information the DNS server has.
dig any <domain.ltd> @<IP>

 # DIG - AXFR Zone Transfer
dig AXFR @ns1.inlanefreight.htb inlanefreight.htb

# Counts the subdomains of a domain
dnsenum --dnsserver <IP> --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt <domain.ltd>

# Subdominn Brute Forcing
for sub in $(cat /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done

```


![[Pasted image 20260530202329.png]]