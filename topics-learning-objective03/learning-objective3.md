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

<figure><img src="../.gitbook/assets/image (17) (1).png" alt=""><figcaption></figcaption></figure>

#### Lista tutti i computer in Dev-Ops OU

```powershell
Get-DomainOU | select -ExpandProperty name
```

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

```powershell
(Get-DomainOU -Identity DevOps).distinguishedname | %{Get-DomainComputer -SearchBase $_} | select name
```

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

#### Lista di tutte le GPO

```powershell
Get-DomainGPO
```

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

#### Prima di poter recuperare le GPO del Gruppo DevOps dobbiamo recuperare il gplink associato al gruppo

```powershell
(Get-DomainOU -Identity DevOps).gplink
```

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

#### Recupero GPO DevOps

```powershell
Get-DomainGPO -Identity '{0BF8D01C-1F62-4BDC-958C-57140B67D147}'
```

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

#### **Enumerate ACLs for the Applocker and DevOps GPOs (AGGIUNGI PARTE BLOODHOUND)**

