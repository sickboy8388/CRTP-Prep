# Domain Enumeration - Trust Enumeration

* Mapping dei trust di dominio attuale

```powershell
Get-DomainTrust (PowerView)
```

* Mapping dei trust di un dominio arbitrario fornito in input

```powershell
Get-DomainTrust -Domain us.dollarcorp.moneycorp.local (PowerView)
```

* Mapping dei trust di dominio attuale

```powershell
Get-ADTrust (Active Directory Module)
```

* Mapping dei trust di un dominio arbitrario fornito in input

```powershell
Get-ADTrust -Identity us.dollarcorp.moneycorp.local (Active Directory Module)
```
