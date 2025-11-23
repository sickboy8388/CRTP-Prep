# User

#### Ottenere lista utenti dominio attuale

```powershell
Get-DomainUser (PowerView)
Get-DomainUser -Identity student1(PowerView)
```

```powershell
Get-ADUser -Filter * -Properties * (ActiveDirectory Module)
Get-ADUser -Identity student1 -Properties * (ActiveDirectory Module)
```

#### Ottenere lista di tutte le proprieta' degli utenti nel dominio attuale

```powershell
Get-DomainUser -Identity student1 -Properties * (PowerView) 
Get-DomainUser -Properties samaccountname,logonCount  (PowerView)
```

{% code overflow="wrap" fullWidth="false" %}
```powershell
Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -MemberType *Property | select Name (ActiveDirectory Module)

Get-ADUser -Filter * -Properties * | select name,logoncount,@{expression={[datetime]::fromFileTime($_.pwdlastset)}} (ActiveDirectory Module)
```
{% endcode %}

#### Ricerca di una stringa arbitraria negli user attributes

```powershell
Get-DomainUser -LDAPFilter "Description=*built*" |
Select name,Description (PowerView)
```

```powershell
Get-ADUser -Filter 'Description -like "*built*"' -
Properties Description | select name,Description (ActiveDirectory Module)
```
