# Domain Enumeration - User Hunting2

* Trova gli host/computer dove un domain admin (o un utente/gruppo specificato in input) ha sessione/i attiva/e

```powershell
Find-DomainUserLocation -Verbose (Check Domain Admins Session)
Find-DomainUserLocation -UserGroupIdentity "RDPUsers" (Check RDPUsers group Session) 
```

* Questa funzione interroga il DC del domino current per i membri di un dato gruppo (ritorna Domain Admins di default come gruppo) utilizzando (**`Get-DomainGroupMember`**), ottiene una lista di host/computer usando (`Get-DomainComputer`) e lista le sessioni e gli utenti attualmente autenticati di ogni host/computer usando `GetNetSession/Get-NetLoggedon`
* Notare che per i Server 2019 e successivi, privilegi amministrativi locali sono richiesti per listare le sessioni

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
