# Priv Esc - Kerberoasting - Set SPN1

* Con i giusti permessi (GenericAll/GenericWrite), un SPN per il target user puo' essere settato (purche' sia unico nel dominio)
* E' possibile successivamente richiedere un TGS senza avere privliegi speciali. Il TGS ottenuto puo' essere Kerberoastato

#### Enumeration RDPUsers Group&#x20;

* PowerView

```powershell
Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "RDPUsers"}
```

#### Check se l'utente ha gia SPN associati

* PowerView

```powershell
Get-DomainUser -Identity <UTENTE> | select serviceprincipalname
```

* Active Directory Module

```powershell
Get-ADUser -Identity supportuser -Properties ServicePrincipalName | select ServicePrincipalName
```
