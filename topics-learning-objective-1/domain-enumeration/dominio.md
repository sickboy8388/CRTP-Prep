# Dominio

## <mark style="color:$info;">POWERVIEW</mark>

#### Ottenere informazioni rispetto il dominio attuale&#x20;

```powershell
Get-Domain (PowerView)
```

#### **Ottenere informazioni rispetto agli oggetti di un altro dominio**&#x20;

```powershell
Get-Domain -Domain moneycorp.local (PowerView)
```

#### Otttenere il SID di dominio per l'attuale dominio

```powershell
Get-DomainSID (PowerView)
```

## <mark style="color:$warning;">ACTIVE DIRECTORY MODULE</mark>

#### Ottenere informazioni rispetto il dominio attuale&#x20;

```powershell
Get-ADDomain (ActiveDirectory Module)
```

#### **Ottenere informazioni rispetto agli oggetti di un altro dominio**&#x20;

```powershell
Get-ADDomain -Identity moneycorp.local (ActiveDirectory Module)
```

#### Otttenere il SID di dominio per l'attuale dominio

```powershell
(Get-ADDomain).DomainSID (ActiveDirectory Module)
```
