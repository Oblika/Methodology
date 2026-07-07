Ky seksion tregon si vulnerabilitetet lidhën me njëra-tjetrën:
```
ATTACK CHAIN — Full Compromise

14:30  Recon: nmap zbuloi Apache 2.4.49 (CVE-2021-41773)
14:32  Exploitation: RCE → shell si www-data
14:45  PrivEsc: sudo misconfiguration → root
14:52  Pillaging: gjendën kredencialet e DB në /var/www/.env
15:05  Lateral Movement: kredencialet punuan në db-server.internal
15:20  Data Exfil: dump i tabelës users (45,000 rekorde)
15:30  AD Compromise: hash i domain admin-it → DCSync → domain pwned

Kohë totale nga zero akses deri te Domain Admin: 60 minuta
```