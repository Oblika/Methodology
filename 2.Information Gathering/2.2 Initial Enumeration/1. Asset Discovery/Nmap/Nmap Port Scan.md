
```bash
# Port scan from service name
nmap 192.168.1.1 -p http, https

# Specific port scan
nmap 192.168.1.1 -p 80,9001,22

# All ports
nmap 192.168.1.1 -p-

# Fast scan 100 ports
nmap -F 192.168.1.1

# Scan top ports
nmap 192.168.1.1 -top-ports 200

#Fast Scan For Usual UDP Ports
nmap 192.168.1.1  -sU

#Scan Only 10 Ports
sudo nmap 10.129.2.28 --top-ports=10 

#Save Nmap Result in a File
nmap 192.168.1.1 -p- -oA target

#Show Number of Open TCP Port
sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l
```