# DomainGroups2

### <mark style="color:$info;">**POWERVIEW**</mark>

* Ottieni tutti i <kbd>membri</kbd> del gruppo <kbd>Domain Admins</kbd>&#x20;

```powershell
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

* Ottiene  tutti i <kbd>gruppi di apparentenza</kbd> dell' <kbd>utente fornito in input</kbd>

```powershell
Get-DomainGroup -UserName "student1"
```

### <mark style="color:$warning;">ACTIVE DIRECTORY MODULE</mark>

* Ottieni tutti i <kbd>membri</kbd> del gruppo <kbd>Domain Admins</kbd>&#x20;

```powershell
Get-ADGroupMember -Identity "Domain Admins" -Recursive
```

* Ottiene  tutti i <kbd>gruppi di apparentenza</kbd> dell' <kbd>utente fornito in input</kbd>&#x20;

```powershell
Get-ADPrincipalGroupMembership -Identity student1
```
