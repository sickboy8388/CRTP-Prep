# Dominio

## POWERVIEW

#### Ottenere informazioni rispetto il dominio attuale

```powershell
Get-Domain (PowerView)
```

#### **Ottenere informazioni rispetto agli oggetti di un altro dominio**

```powershell
Get-Domain -Domain moneycorp.local (PowerView)
```

#### Otttenere il SID di dominio per l'attuale dominio

```powershell
Get-DomainSID (PowerView)
```

## ACTIVE DIRECTORY MODULE

#### Ottenere informazioni rispetto il dominio attuale

```powershell
Get-ADDomain (ActiveDirectory Module)
```

#### **Ottenere informazioni rispetto agli oggetti di un altro dominio**

```powershell
Get-ADDomain -Identity moneycorp.local (ActiveDirectory Module)
```

#### Otttenere il SID di dominio per l'attuale dominio

```powershell
(Get-ADDomain).DomainSID (ActiveDirectory Module)
```
