# Trust Abuse - MSSQL Servers

* MSSQL Server sono presenti all'interno di domini windows
* SQLServer fornisce una buona opzione per movimenti laterali
* Il tool utilizzato per enumerare e' PowerUPSql, [https://github.com/NetSPI/PowerUpSQL](https://github.com/NetSPI/PowerUpSQL)

#### DISCOVERY

```powershell
Get-SQLInstanceDomain
```

#### CHECK ACCESSIBILITA'

```powershell
Get-SQLConnectionTestThreaded
```

```powershell
Get-SQLInstanceDomain | Get-SQLConnectionTestThreaded -Verbose
```

#### GATHER INFORMATION

```powershell
Get-SQLInstanceDomain | Get-SQLServerInfo -Verbose
```
