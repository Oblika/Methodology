
```bash
# Install NetExec
sudo apt-get -y install netex

# Syntax
netexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist

# Test WinRM authentication with user & password lists
netexec winrm 10.129.42.197 -u user.list -p password.list
```