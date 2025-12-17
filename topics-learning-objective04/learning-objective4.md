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
Get-DomainTrust -Domain dollarcorp.moneycorp.local
```

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

#### Mappare gli external-trust di moneycorp.local

```
// Some code
```
