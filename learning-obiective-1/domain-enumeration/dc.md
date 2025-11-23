# DC

#### Ottenere DC dell'attuale dominio

```powershell
Get-DomainController (PowerView)
```

```powershell
 Get-ADDomainController(ActiveDirectory Module)
```

#### Ottenere DC di un altro dominio

&#x20;

```powershell
Get-DomainController -Domain moneycorp.local (Powerview)
```

```powershell
Get-ADDomainController -DomainName moneycorp.local -Discover (ActiveDirectory Module)
```
