
Kerberos provides different error messages for valid and invalid usernames, enabling user enumeration without authentication.


#### Using kerbrute
```bash
# Using kerbrute
kerbrute userenum --dc target.com -d DOMAIN.COM users.txt
```

#### Using Nmap Scripts
```bash
# Using Nmap
nmap -p 88 --script krb5-enum-users --script-args krb5-enum-users.realm='DOMAIN.COM',userdb=users.txt target.com
```

#### Manual Username Enumeration
```bash
# Manual enumeration
for user in $(cat users.txt); do
  getTGT.py DOMAIN/$user -dc-ip target.com -no-pass 2>&1 | grep -v "KDC_ERR_PREAUTH_REQUIRED"
done
```

