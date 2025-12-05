# Host/Computers

### POWER VIEW

• Ottieni la lista degli <kbd>host</kbd> presenti nell'attuale dominio

```powershell
Get-DomainComputer | select Name (PowerView)
```

• Ottieni la lista degli <kbd>host</kbd> presenti con OS <kbd>Windows Server2022</kbd> rispetto l'attuale dominio

```powershell
Get-DomainComputer -OperatingSystem "Server 2022" (PowerView)
```

• Ottieni la lista degli <kbd>host</kbd> presenti effettuando richieste <kbd>ICMP ECHO</kbd> rispetto gli host afferenti l'attuale dominio

```powershell
Get-DomainComputer -Ping (PowerView)
```

### **ACTIVE DIRECTORY MODULE**

• Ottieni la lista degli <kbd>host</kbd> presenti con OS <kbd>Windows Server2022</kbd> rispetto l'attuale dominio

```powershell
Get-ADComputer -Filter * | select Name (ActiveDirectory Module)
```

• Ottieni la lista degli <kbd>host</kbd> presenti e le rispettive <kbd>Properties</kbd>

```powershell
Get-ADComputer -Filter * -Properties * (ActiveDirectory Module)
```

• Ottieni la lista degli <kbd>host</kbd> presenti con OS <kbd>Windows Server2022</kbd> rispetto l'attuale dominio

```powershell
 Get-ADComputer -Filter 'OperatingSystem -like "Server 2022"' - Properties OperatingSystem | select Name,OperatingSystem (ActiveDirectory Module)
```

• Ottieni la lista degli <kbd>host</kbd> presenti effettuando richieste <kbd>ICMP ECHO</kbd> rispetto gli host afferenti l'attuale dominio

```powershell
Get-ADComputer -Filter * -Properties DNSHostName | %{Test- Connection -Count 1 -ComputerName $_.DNSHostName} (ActiveDirectory Module)
```
