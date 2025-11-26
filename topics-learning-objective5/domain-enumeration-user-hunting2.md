# Domain Enumeration - User Hunting2

* Trova gli host/computer dove un domain admin (o un utente/gruppo specificato in input) ha sessione/i attiva/e

```powershell
Find-DomainUserLocation -Verbose (Check Domain Admins Session)
Find-DomainUserLocation -UserGroupIdentity "RDPUsers" (Check RDPUsers group Session) 
```

* Questa funzione interroga il DC del domino current per i membri di un dato gruppo (ritorna Domain Admins di default come gruppo) utilizzando (**Get-DomainGroupMembe**r), ottiene una lista di host/computer usando (Get-DomainComputer) e lista le sessioni e gli utenti attualmente autenticati di ogni host/computer usando GetNetSession/Get-NetLoggedon

