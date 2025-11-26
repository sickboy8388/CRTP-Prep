# Domain Enumeration - User Hunting3

* Trova gli host/computer dove una sessione admin e' disponibile e l'utente attuale ha accesso admin (utilizza `Test-AdminAccess`)

```powershell
Find-DomainUserLocation -CheckAccess (PowerView)
```

* Trova host/computer (File Servers e Distributed File servers) dove una domain admin session e' disponibile

```powershell
Find-DomainUserLocation -Stealth
```

* Lista le sessioni su macchine remote ([https://github.com/Leo4j/Invoke-\
  SessionHunter](https://github.com/Leo4j/Invoke-SessionHunter))

```powershell
Invoke-SessionHunter -FailSafe
```

* Il comando precedente `non ha bisogno di accesso admin` sulla macchina remota, utilizza `Remote Registry and interroga l'hive HKEY_USERS`
* Un `comando OPSEC friendly` sarebbe (evitando di connettersi a tutte le macchine target specificate nella lista tramite l'opzione `-Targets` )

```powershell
Invoke-SessionHunter -NoPortScan -Targets C:\AD\Tools\servers.txt
```
