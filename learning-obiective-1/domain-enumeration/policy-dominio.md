# Policy Dominio

## <mark style="color:$info;">**POWERVIEW**</mark>

#### Ottenere policy dell'attuale dominio

```powershell
Get-DomainPolicyData (PowerView)
```

## <mark style="color:$warning;">**ACTIVE DIRECTORY MODULE**</mark>

#### Ottenere policy dell'attuale dominio

```powershell
(Get-DomainPolicyData).systemaccess (ActiveDirectory Module)
```

#### Ottenere policy di un altro dominio

```powershell
(Get-DomainPolicyData).systemaccess (ActiveDirectory Module)
```

