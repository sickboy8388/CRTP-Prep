# Domain Enumeration - OU

* Le organizzazioni utilizzano le  Organizational Units (OUs) per delegare processi amminstrativi
* Una OU e' il livello piu' basso in termini di AD container a cui una GPO puo'essere applicata.
* Attacchi a livello OU a causa di misconfiguration o permessi eccessivi in termini di GPO sono tra i piu' comuni in termini di ambienti enterprise.
* Ottenere le OUs del dominio

```powershell
Get-DomainOU (PowerView)
Get-ADOrganizationalUnit -Filter * -Properties * (ActiveDirectory Module)
```

* Ottenere le GPS applicata ad una OU. (Leggere <kbd>GPOname</kbd> dall'attributo <kbd>gplink</kbd> di <kbd>Get-NetOU</kbd> )

```powershell
Get-DomainGPO -Identity "{0D1CC23D-1F20-4EEE-AF64-D99597AE2A6E}" (PowerView)
```
