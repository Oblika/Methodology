```bash
# Shiko biletat Kerberos aktuale në sesion
klist

# Importo PowerView për skanimin e Active Directory
Import-Module .\\PowerView.ps1

# Provoni të merrni info duke përdorur kredencialet manualisht
Get-DomainUser -SPN -Credential $Cred

# Regjistro konfigurim të ri për PSSession (duhet të jesh në hostin lokal)
Register-PSSessionConfiguration -Name mySession -RunAsCredential (Get-Credential)

# Rinis WinRM (mund të bjerë sesioni aktual)
Restart-Service WinRM

# Lidhu në sesionin e ri
Enter-PSSession -ComputerName target-host -ConfigurationName mySession -Credential DOMAIN\\user
```