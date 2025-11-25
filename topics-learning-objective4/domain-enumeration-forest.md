# Domain Enumeration - Forest

### **Mapping Foreste**

* Ottenere dettagli foresta attuale

```powershell
Get-Forest (PowerView)
Get-ADForest (Active Directory Module)
```

* Ottenere dettagli foresta arbitraria fornita in input

```powershell
Get-Forest -Forest eurocorp.local (PowerView)
Get-ADForest -Identity eurocorp.local (Active Directory Module)
```

* Ottenere tutti i domini della foresta attuale

```powershell
Get-ForestDomain (PowerView)
(Get-ADForest).Domains (Active Directory Module)
```

* Ottenere tutti i domini della foresta fornita in input

```powershell
Get-ForestDomain -Forest eurocorp.local (PowerView)
```

* Ottenere tutti i global catalog della foresta attuale

```powershell
Get-ForestGlobalCatalog (PowerView)
Get-ADForest | select -ExpandProperty GlobalCatalogs (Active Directory Module)
```

* Ottenere tutti i global catalog della foresta fornita in input

```powershell
Get-ForestGlobalCatalog -Forest eurocorp.local (PowerView)
```

* Mappa i trust della foresta attuale (NON PRESENTI NEL LAB)

```powershell
Get-ForestTrust (PowerView)
Get-ADTrust -Filter 'msDS-TrustForestTrustInfo -ne "$null"' (Active Directory Module)
```

* Mappa i trust di una foresta fornita in input(NON PRESENTI NEL LAB)

```powershell
Get-ForestTrust -Forest eurocorp.local (PowerView)
```
