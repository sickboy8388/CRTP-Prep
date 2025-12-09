# Domain Enumeration - User Hunting1

* Trova tutte le macchine del dominio attuale dove l'utente attuale ha local admin access

```powershell
Find-LocalAdminAccess -Verbose
```

* Questa funzione interroga a il DC del domino corrente o di quello fornito in input al fine di ottenere una lista di host/computer (**`Get-NetComputer`**) e poi su ogni host usa **`Invoke-CheckLocalAdminAccess`** con multi-thread su ogni host/computer ritornato da **`Get-NetComputer`**
* E' possibile ottenere lo stesso risultato con l'aiuto di tool di amministrazione remota come WMI and Powershell (remoting). Molto utile in caso di porte bloccate (RPC e SMB) usate da **`Find-LocalAdminAccess`**
* Le funzioni utili in quel caso sono **`Find-WMILocalAdminAccess.ps1`** e **`Find-PSRemotingLocalAdminAccess.ps1`**

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
