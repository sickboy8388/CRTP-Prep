# User

#### Ottenere lista utenti dominio attuale

```
Get-DomainUser (PowerView)
Get-DomainUser -Identity student1(PowerView)
```

```
Get-ADUser -Filter * -Properties * (ActiveDirectory Module)
Get-ADUser -Identity student1 -Properties * (ActiveDirectory Module)
```

#### Ottenere lista di tutte le proprieta' degli utenti nel dominio attuale

```
Get-DomainUser -Identity student1 -Properties * (PowerView) 
Get-DomainUser -Properties samaccountname,logonCount  (PowerView)
```

{% code overflow="wrap" fullWidth="false" %}
```
Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -MemberType *Property | select Name (ActiveDirectory Module)

Get-ADUser -Filter * -Properties * | select name,logoncount,@{expression={[datetime]::fromFileTime($_.pwdlastset)}} (ActiveDirectory Module)
```
{% endcode %}

#### Ricerca di una stringa arbitraria negli user attributes

<kbd>**`Get-DomainUser`**</kbd>

