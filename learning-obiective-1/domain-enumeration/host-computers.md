# Host/Computers

• Ottieni la lista degli <kbd>host</kbd> presenti nell'attuale dominio&#x20;

### <mark style="color:$info;">POWER VIEW</mark>

```powershell
Get-DomainComputer | select Name (PowerView)
```

• Ottieni la lista degli <kbd>host</kbd> presenti con OS <kbd>Windows Server2022</kbd> rispetto l'attuale dominio&#x20;

```powershell
Get-DomainComputer -OperatingSystem "Server 2022" (PowerView)
```

• Ottieni la lista degli <kbd>host</kbd> presenti effettuando richieste <kbd>ICMP ECHO</kbd> rispetto gli host afferenti  l'attuale dominio&#x20;

```powershell
Get-DomainComputer -Ping (PowerView)
```

### <mark style="color:$warning;">**ACTIVE DIRECTORY MODULE**</mark>

```powershell
Get-ADComputer -Filter * | select Name (ActiveDirectory Module)
```

```powershell
Get-ADComputer -Filter * -Properties * (ActiveDirectory Module)
```

```powershell
 Get-ADComputer -Filter 'OperatingSystem -like "Server 2022"' - Properties OperatingSystem | select Name,OperatingSystem (ActiveDirectory Module)
```

```powershell
Get-ADComputer -Filter * -Properties DNSHostName | %{Test- Connection -Count 1 -ComputerName $_.DNSHostName} (ActiveDirectory Module)
```

