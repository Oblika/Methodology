
```bash
#Default Scripts
sudo nmap <target> -sC

#Specific Scripts Category
sudo nmap <target> --script <category>

#Multi Scripts
sudo nmap <target> --script <script-name>,<script-name>,...

#Specifying Scripts
sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands

#Aggressive Scan
sudo nmap 10.129.2.28 -p 80 -A

#Vuln Category
sudo nmap 10.129.2.28 -p 80 -sV --script vuln 
```