# LEARNING OBJECTIVE3

<div data-full-width="true"><figure><img src="../.gitbook/assets/Schermata da 2025-11-25 16-31-44.png" alt=""><figcaption></figcaption></figure></div>

* Enumerate following for the dollarcorp domain:
  * List all the OUs
  * List all the computers in the DevOps OU
  * List the GPOs
  * Enumerate GPO applied on the DevOps OU
  * Enumerate ACLs for the Applocker and DevOps GPOs

#### Lista tutte le OU

```powershell
Get-DomainOU
```

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

#### Lista tutti i computer in Dev-Ops OU

```powershell
Get-DomainOU | select -ExpandProperty name
```

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

```powershell
(Get-DomainOU -Identity DevOps).distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name
```

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>
