# LEARNING OBJECTIVE4

<figure><img src="../.gitbook/assets/Schermata da 2025-11-25 17-50-57.png" alt=""><figcaption></figcaption></figure>

* &#x20;Enumerate all domains in the moneycorp.local forest.
* &#x20;Map the trusts of the dollarcorp.moneycorp.local domain.
* &#x20;Map External trusts in moneycorp.local forest.
* &#x20;Identify external trusts of dollarcorp domain. Can you enumerate trusts\
  for a trusting forest?

#### Enumerare tutti i domini presenti nella Foresta&#x20;

```powershell
Get-ForestDomain
```

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

#### Mappare i trust di dollarcorp.moneycorp.local

```powershell
Get-DomainTrust
```

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

#### Mappare gli external-trust di moneycorp.local

```powershell
 Get-DomainTrust | ?{$_.TrustAttributes -eq "FILTER_SIDS"}
```

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

#### Enumerazione dei trust di una Foresta Trusted

```powershell
 Get-ForestDomain -Forest eurocorp.local | %{Get-DomainTrust -Domain $_.name}
```

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

E'possibile recuperare tale informazione poiche il trust tra dollarcorp.moneycorp.local e eurocorp.local e' bi-direzionale
