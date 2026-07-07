

```bash
# Kontrollojm nese ky user ka privilegje ne smb
netexec smb 10.10.11.42 -u olivia -p ichliebedich

# Shikojm nese ka privilegje ne winrm
netexec winrm 10.10.11.42 -u olivia -p ichliebedich

# Kemi privilegje per FTP ose jo me kte user
netexec ftp 10.10.11.42 -u olivia -p ichliebedich

netexec mssql 10.129.228.253 -u sql_svc -p REGGIE1234ronnie 
```