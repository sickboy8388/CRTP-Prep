# Priv Esc - Kerberoasting - Powerview/AD Module

#### Trova user account utilizzati dai Service Accounts

* Powerview

```powershell
Get-DomainUser -SPN
```

* Active Directory module

```powershell
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```
