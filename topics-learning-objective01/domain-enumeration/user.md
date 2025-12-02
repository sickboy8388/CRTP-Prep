# User

## <mark style="color:$info;">POWERVIEW</mark>

#### Ottenere lista utenti dominio attuale e identita specifica di un utente

```powershell
Get-DomainUser (PowerView)
Get-DomainUser -Identity student1(PowerView)
```

#### Ottenere le proprieta' specifiche di un utente e listare di tutte/un sottoinsieme delle proprieta' degli utenti nel dominio attuale

```powershell
Get-DomainUser -Identity student1 -Properties * (PowerView) 
Get-DomainUser -Properties samaccountname,logonCount  (PowerView)
```

#### Ricerca di una stringa arbitraria negli user attributes

```powershell
Get-DomainUser -LDAPFilter "Description=*built*" |
Select name,Description (PowerView)
```

## <mark style="color:$warning;">ACTIVE DIRECTORY MODULE</mark>

#### Ottenere lista utenti dominio attuale e identita specifica di un utente

```powershell
Get-ADUser -Filter * -Properties * (ActiveDirectory Module)
Get-ADUser -Identity student1 -Properties * (ActiveDirectory Module)
```

#### Ottenere le proprieta' specifiche di un utente e listare di tutte/un sottoinsieme delle proprieta' degli utenti nel dominio attuale

{% code overflow="wrap" fullWidth="false" %}
```powershell
Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -MemberType *Property | select Name (ActiveDirectory Module)

Get-ADUser -Filter * -Properties * | select name,logoncount,@{expression={[datetime]::fromFileTime($_.pwdlastset)}} (ActiveDirectory Module)
```
{% endcode %}

#### Ricerca di una stringa arbitraria negli user attributes

```powershell
Get-ADUser -Filter 'Description -like "*built*"' -
Properties Description | select name,Description (ActiveDirectory Module)
```

