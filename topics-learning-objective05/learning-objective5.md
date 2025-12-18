# LEARNING OBJECTIVE5

<figure><img src="../.gitbook/assets/Schermata da 2025-11-26 14-43-31.png" alt=""><figcaption></figcaption></figure>

* Exploita un servizio su dcorp-studentx e eleva i privliegi ad amminstratore locale
* Identifica un host nel dominio dove lo studentex ha accesso amministrativo locale
* Utilizzando i privilegi dell'utente del servizio Jenkins su 172.16.3.11:8080, ottenere i privilegi su 172.16.3.11, il dcorp-ci server

#### Exploit servizio su studentVM (dcorp-std178)

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

