# Groups

### <mark style="color:$info;">**POWERVIEW**</mark>

* Ottieni tutti i <kbd>gruppi</kbd> del <kbd>Dominio</kbd> di appartenenza

```powershell
Get-DomainGroup | select Name
```

* Ottieni tutti i <kbd>gruppi</kbd> del <kbd>Dominio fornito in input</kbd>

```powershell
Get-DomainGroup -Domain <targetdomain>
```

* Ottieni tutti i <kbd>gruppi</kbd> che contengono la parola <kbd>Admin</kbd> nel nome del gruppo

```powershell
Get-DomainGroup *admin*
```

### <mark style="color:$warning;">ACTIVE DIRECTORY MODULE</mark>

* Ottieni tutti i <kbd>gruppi</kbd> del <kbd>Dominio</kbd> di appartenenza

```powershell
Get-ADGroup -Filter * | select Name
```

* Ottieni tutti i <kbd>gruppi</kbd> e le rispettiva <kbd>Proprieta'</kbd>

```powershell
Get-ADGroup -Filter * -Properties *
```

* Ottieni tutti i <kbd>gruppi</kbd> che contengono la parola <kbd>Admin</kbd> nel nome del gruppo

```powershell
Get-ADGroup -Filter 'Name -like "*admin*"' | select Name
```

