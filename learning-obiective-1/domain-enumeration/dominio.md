# Dominio

#### Ottenere informazioni rispetto il dominio attuale&#x20;

```powershell
Get-Domain (PowerView)
```

```powershell
Get-ADDomain (ActiveDirectory Module)
```

#### **Ottenere informazioni rispetto agli oggetti di un altro dominio**&#x20;

```powershell
Get-Domain -Domain moneycorp.local (PowerView)
```

```powershell
Get-ADDomain -Identity moneycorp.local (ActiveDirectory Module)
```

#### Otttenere il SID di dominio per l'attuale dominio

```powershell
Get-DomainSID (PowerView)
```

```powershell
(Get-ADDomain).DomainSID (ActiveDirectory Module)
```
