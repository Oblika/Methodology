
```bash
# Requested scan (including ping scans) use tiny fragmented IP packets. Harder for packet filters
nmap -f 192.168.1.1

#Disables DNS resolution.
nmap -n 192.168.1.1

#Scans the target by using 5 random different source IP address.
nmap -D RND:5 192.168.1.1

#Disables ARP ping
nmap  192.168.1.1 --disable-arp-ping

# Set your own offset size(8, 16, 32, 64)
nmap 192.168.1.1 --mtu 32

# Send scans from spoofed IPs
nmap 192.168.1.1 -D 192.168.1.11, 192.168.1.12, 192.168.1.13, 192.168.1.13 

#We use -D RND:5 to add 5 random IP to hide our's
sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5
```
