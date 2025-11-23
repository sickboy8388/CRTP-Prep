# Host/Computers

• Ottieni la lista degli host presenti nell'attuale dominio&#x20;

```powershell
Get-DomainComputer | select Name Get-DomainComputer -OperatingSystem "Server 2022" (PowerView)
```

```powershell
Get-DomainComputer -Ping Get-ADComputer -Filter * | select Name (PowerView)
```

```powershell
Get-ADComputer -Filter * -Properties * Get-ADComputer -Filter 'OperatingSystem -like "Server 2022"' - Properties OperatingSystem | select Name,OperatingSystem (ActiveDirectory Module)
```

```powershell
Get-ADComputer -Filter * -Properties DNSHostName | %{Test- Connection -Count 1 -ComputerName $_.DNSHostName} (ActiveDirectory Module)
```
