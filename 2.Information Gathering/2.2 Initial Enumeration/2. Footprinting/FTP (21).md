
```bash
# Default Configuration
sudo apt install vsftpd 
cat /etc/vsftpd.conf | grep -v "#"

# Connect to FTP (Anonymous Authentication)
ftp <IP>

# Recursive Listing
ftp> ls -R

# Interact with a service on the target.
nc -nv <IP> <PORT>

# Download all available files on the target FTP server
wget -m --no-passive ftp://anonymous:anonymous@<IP>

# After download all file we see in tree format in our host
tree .

# Interact with a service on the target.
telnet <IP> <PORT>

# Nmap FTP Scripts
sudo nmap --script-updatedb

# Upload a File
touch testupload.txt

# Checks if the FTP server supports TLS/SSL certificate
openssl s_client -connect <IP>:<PORT> -starttls ftp

# Shikojm te gjitha skriptet NSE qe jan ne dispozicion
find / -type f -name ftp* 2>/dev/null | grep scripts
```



|**Commands**|**Description**|
|---|---|
|`connect`|Sets the remote host, and optionally the port, for file transfers.|
|`get`|Transfers a file or set of files from the remote host to the local host.|
|`put`|Transfers a file or set of files from the local host onto the remote host.|
|`quit`|Exits tftp.|
|`status`|Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on.|
|`verbose`|Turns verbose mode, which displays additional information during file transfer, on or off.|
