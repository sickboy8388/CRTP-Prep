# DC

## POWERVIEW

#### Ottenere DC dell'attuale dominio

```powershell
Get-DomainController (PowerView)
```

#### Ottenere DC di un altro dominio

```powershell
Get-DomainController -Domain moneycorp.local (Powerview)
```

## **ACTIVE DIRECTORY MODULE**

#### Ottenere DC dell'attuale dominio

```powershell
 Get-ADDomainController(ActiveDirectory Module)
```

#### Ottenere DC di un altro dominio

```powershell
Get-ADDomainController -DomainName moneycorp.local -Discover (ActiveDirectory Module)
```
