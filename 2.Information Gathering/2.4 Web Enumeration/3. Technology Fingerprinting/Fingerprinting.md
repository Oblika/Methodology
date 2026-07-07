
```bash
# Identifies a wide range of web technologies, including CMSs, frameworks, analytics tools, and more
Wappalyzer <browser extension>

# Offers both free and paid plans with varying levels of detail.
BuiltWith

# Uses a vast database of signatures to identify various web technologies.
WhatWeb <domain>

# Can be used with scripts (NSE) to perform more specialised fingerprinting.
Nmap

# Provides detailed reports on a website's technology, hosting provider, and security posture.
Netcraft

# wafw00f installation
pip3 install git+https://github.com/EnableSecurity/wafw00f
wafw00f inlanefreight.com

# Helps determine if a WAF is present and, if so, its type and configuration.
wafw00f <domian.ltd>

# Baner Grabbing
curl -I inlanefreight.com

# Nikto installation
sudo apt update && sudo apt install -y perl
git clone https://github.com/sullo/nikto
cd niktro/program 
chmod +x ./nikto.pl

# Nikto is a powerful open-source web server scanner, primary function as a vulnerability assessment tool

nikto -h <domain.ltd> -Tuning b

view-source
```


Identify:  
- CMS  
- Framework  
- Libraries  
- Server software  
- Programming language  
- Version numbers