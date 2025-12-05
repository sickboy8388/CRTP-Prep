# Policy Dominio

## **POWERVIEW**

#### Ottenere policy dell'attuale dominio

```powershell
Get-DomainPolicyData (PowerView)
```

## **ACTIVE DIRECTORY MODULE**

#### Ottenere policy dell'attuale dominio

```powershell
(Get-DomainPolicyData).systemaccess (ActiveDirectory Module)
```

#### Ottenere policy di un altro dominio

```powershell
(Get-DomainPolicyData).systemaccess (ActiveDirectory Module)
```
