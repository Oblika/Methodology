
```bash
# Disable port scanning. Host discovery only.
nmap -sn 192.168.1.1

# Disable host discovery. Port scan only.
nmap -Pn 192.168.1.1

# Never do DNS resolution
nmap -n 192.168.1.1

#Scan ICMP Echo Requests (Ping)
nmap 192.168.1.1 -PE

# Scan a file with IP List
nmap -sC -sV -A -iL IP_List

# Show only IP Address
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5

#Scan IP in hosts.lst
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5

sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l
```