```bash
# Listo trustet me AD module
Import-Module ActiveDirectory
Get-ADTrust -Filter *

# Listo trustet me PowerView
Import-Module Import-Module .\\PowerView
Get-DomainTrust
Get-DomainTrustMapping

# Listo user nga një domain i besuar
Get-DomainUser -Domain <domain> | select SamAccountName

# netdom (cmd)
# trust → liston trustet.
netdom query /domain:<domain> trust
# dc → kontrolluesit e domain-it.
netdom query /domain:<domain> dc 
# workstation → stacionet e punës dhe serverat në domain.
netdom query /domain:<domain> workstation
```